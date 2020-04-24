---
title: Virtualizzazione sicura di Active Directory Domain Services (AD DS)
description: Rollback degli USN e virtualizzazione sicura di Active Directory
ms.topic: article
ms.prod: windows-server
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: 25a5c2222f50b37bff2bcfe41184d6d9fa35995c
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "77465505"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>Virtualizzazione sicura di Active Directory Domain Services (AD DS)

>Si applica a: Windows Server

A partire da Windows Server 2012, Active Directory Domain Services offre maggiore supporto per la virtualizzazione dei controller di dominio grazie all'introduzione di funzionalità compatibili con la virtualizzazione. Questo articolo illustra il ruolo svolto da USN e ID chiamata nella replica dei controller di dominio e descrive alcuni problemi che possono verificarsi.

## <a name="update-sequence-number-and-invocationid"></a>Numero di sequenza di aggiornamento e ID chiamata

Gli ambienti virtualizzati pongono sfide straordinarie per i carichi di lavoro distribuiti che dipendono da uno schema di replica basato su clock logico. La replica di Servizi di dominio Active Directory, ad esempio, utilizza un valore con incremento monotono, denominato numero di sequenza di aggiornamento o USN, assegnato alle transazioni in ogni controller di dominio. Istanza del ogni controller di dominio database viene inoltre assegnata un'identità, denominata ID chiamata. L'ID chiamata di un controller di dominio e il relativo USN fungono insieme da identificatore univoco associato a ogni transazione di scrittura eseguita in ogni controller di dominio e devono essere univoci all'interno della foresta.

La replica di Servizi di dominio Active Directory utilizza l'ID chiamata e gli USN in ogni controller di dominio per determinare le modifiche che è necessario replicare in altri controller di dominio. Se un controller di dominio verrà riportato nel tempo all'esterno di presenza del controller di dominio e viene riutilizzato un USN per una transazione interamente diversa, la replica non convergerà perché altri controller di dominio verrà ritiene di che avere già ricevuto l'aggiornamento associato all'USN riutilizzato nel contesto di tale ID.

Nell'illustrazione seguente, ad esempio, viene mostrata la sequenza di eventi che si verifica in Windows Server 2008 R2 e nei sistemi operativi precedenti quando il rollback degli USN viene rilevato in VDC2, ovvero il controller di dominio di destinazione eseguito in una macchina virtuale. In questa illustrazione, il rilevamento del rollback degli USN si verifica su VDC2 quando un partner di replica rileva che VDC2 ha inviato un valore USN di aggiornamento che è stato individuato in precedenza dal partner di replica, a indicare che il database di VDC2 ha rollback nel tempo in modo non corretto.

![Sequenza di eventi quando viene rilevato il rollback degli USN](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Una macchina virtuale (VM) semplifica per gli amministratori degli hypervisor ripristinare un dominio USN del controller (il clock logico), ad esempio, l'applicazione di uno snapshot di fuori di presenza del controller di dominio. Per ulteriori informazioni sugli USN e USN rollback, inclusa un'altra illustrazione per illustrare le istanze non rilevate del rollback degli USN, vedere [USN e Rollback degli USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

A partire da Windows Server 2012, il controller di dominio virtuali AD DS ospitato in piattaforme hypervisor che espongono un identificatore denominato ID di generazione VM può rilevare e utilizzare misure di sicurezza necessarie per proteggere l'ambiente di dominio Active Directory se la macchina virtuale è rollback in tempo per l'applicazione di uno snapshot della macchina Virtuale. Questa progettazione utilizza un meccanismo indipendente dal fornitore dell'hypervisor per esporre l'identificatore nello spazio degli indirizzi della macchina virtuale guest, in modo da garantire un'esperienza di virtualizzazione sicura in qualsiasi hypervisor che supporta tale ID di generazione macchina virtuale. Questo identificatore può essere campionato da servizi e applicazioni eseguiti all'interno della macchina virtuale per rilevare se è stato eseguito il rollback di una macchina virtuale in tempo.

## <a name="effects-of-usn-rollback"></a>Effetti del rollback degli USN

Quando si verifica il rollback degli USN, le modifiche apportate agli oggetti e agli attributi non vengono replicate in ingresso dai controller di dominio di destinazione che hanno precedentemente rilevato l'USN.

Poiché i controller di dominio di destinazione ritengono di essere aggiornati, non vengono segnalati errori di replica nei registri eventi di Directory Service o dagli strumenti di monitoraggio e diagnostica.

Il rollback degli USN può influire sulla replica di qualsiasi oggetto o attributo in qualsiasi partizione. L'effetto collaterale osservato più di frequente è che gli account utente e gli account computer creati nel controller di dominio di rollback non sono presenti in uno o più partner di replica oppure gli aggiornamenti delle password originati nel controller di dominio di rollback non sono presenti nei partner di replica.

Un rollback degli USN può impedire la replica di qualsiasi tipo di oggetto in qualsiasi partizione di Active Directory. I tipi di oggetto includono:

* Topologia di replica di Active Directory e relativa pianificazione
* Presenza di controller di dominio nella foresta e dei ruoli inclusi in questi controller
* Presenza di partizioni di dominio e applicazione nella foresta
* Presenza di gruppi di sicurezza e delle relative appartenenze correnti a gruppi
* Registrazione di record DNS in zone DNS integrate in Active Directory

Il problema di sicurezza relativo agli USN può riguardare centinaia, migliaia o addirittura decine di migliaia di modifiche a utenti, computer, trust, password e gruppi di sicurezza. Tale problema viene definito dalla differenza tra il numero USN più elevato presente al momento dell'esecuzione del backup dello stato del sistema ripristinato e il numero di modifiche di origine create nel controller di dominio di rollback prima di essere portato offline.

## <a name="detecting-a-usn-rollback"></a>Rilevamento di un rollback degli USN

Poiché è difficile rilevare un rollback degli USN, un controller di dominio registra l'evento 2095 quando un controller di dominio di origine invia un numero USN precedentemente riconosciuto a un controller di dominio di destinazione senza modifiche corrispondenti nell'ID chiamata.

Per impedire che gli aggiornamenti di origine univoci ad Active Directory vengano creati nel controller di dominio ripristinato in modo errato, il servizio Accesso rete viene messo in pausa. Quando il servizio Accesso rete viene messo in pausa, gli account utente e computer non possono modificare la password in un controller di dominio che non effettuerà la replica in uscita di tali modifiche. Analogamente, gli strumenti di amministrazione di Active Directory preferiranno un controller di dominio integro quando eseguono gli aggiornamenti agli oggetti in Active Directory.

In un controller di dominio, vengono registrati messaggi di evento simili a quelli riportati di seguito se si verificano le condizioni seguenti:

* Un controller di dominio di origine invia un numero USN precedentemente riconosciuto a un controller di dominio di destinazione.
* Non si verifica alcuna modifica corrispondente nell'ID chiamata.

Questi eventi possono essere acquisiti nel registro eventi di Directory Service. Possono essere tuttavia sovrascritti prima di essere osservati da un amministratore.

Se sospetti che si sia verificato un rollback degli USN ma non è visibile un evento corrispondente nei registri eventi, verifica la presenza della voce DSA non scrivibile nel Registro di sistema. Questa voce è la prova ufficiale che si è verificato un rollback degli USN.

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> L'eliminazione o la modifica manuale del valore relativo alla voce del Registro di sistema DSA non scrivibile determina l'impostazione del controller di dominio di rollback in uno stato non supportato in modo permanente. Tali modifiche non sono pertanto supportate. In particolare, la modifica del valore rimuove il comportamento di quarantena aggiunto dal codice di rilevamento del rollback degli USN. Le partizioni di Active Directory nel controller di dominio di rollback saranno incoerenti in modo permanente con i partner di replica diretti e transitivi nella stessa foresta di Active Directory.

Per altre informazioni su questa chiave del Registro di sistema e sulla procedura di risoluzione, vedi l'articolo del supporto [Errore di replica di Active Directory 8456 o 8457: "Il server di origine | destinazione rifiuta attualmente le richieste di replica"](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination).

## <a name="virtualization-based-safeguards"></a>Misure di sicurezza basate sulla virtualizzazione

Durante l'installazione di controller di dominio Active Directory archivia inizialmente l'identificatore di generazione macchina Virtuale come parte dell'attributo msDS-GenerationID sull'oggetto computer del controller di dominio nel proprio database (noto anche come la directory information tree o DIT). L'ID di generazione macchina virtuale viene registrato in modo indipendente da un driver Windows all'interno della macchina virtuale.

Quando un amministratore ripristina la macchina virtuale da uno snapshot precedente, il valore corrente dell'ID di generazione macchina virtuale ottenuto dal driver della macchina virtuale viene confrontato con un valore nel nell'albero delle informazioni di directory.

Se i due valori sono diversi, l'ID chiamata viene reimpostato e il pool di RID viene ignorato impedendo in tal modo il riutilizzo dell'USN. Se i valori sono identici, viene eseguito il commit della transazione come normale.

Servizi di dominio Active Directory confronta inoltre il valore corrente dell'ID di generazione macchina virtuale della macchina virtuale con il valore nell'albero delle informazioni di directory ogni volta che viene riavviato il controller di dominio e, se diverso, reimposta l'ID chiamata, ignora il pool di RID e aggiorna l'albero delle informazioni di directory con il nuovo valore. Sincronizza inoltre in modo non autorevole la cartella SYSVOL per completare il ripristino sicuro. In questo modo le misure di sicurezza possono estendersi all'applicazione di snapshot nelle macchine virtuali arrestate. Queste misure di sicurezza introdotti in Windows Server 2012 consentono agli amministratori di dominio Active Directory beneficiare dei vantaggi univoci di distribuzione e gestione di controller di dominio in un ambiente virtualizzato.

La figura seguente mostra come vengono applicate le misure di sicurezza della virtualizzazione quando viene rilevato lo stesso rollback degli USN in un controller di dominio virtualizzato che esegue Windows Server 2012 in un hypervisor che supporta VM-GenerationID.

![Misure di sicurezza applicate quando viene rilevato lo stesso rollback degli USN](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

In questo caso, quando l'hypervisor rileva una modifica al valore dell'ID di generazione macchina virtuale, vengono attivate misure di sicurezza di virtualizzazione, inclusi la reimpostazione dell'ID chiamata per il controller di dominio virtualizzato (da A a B nell'esempio precedente) e l'aggiornamento del valore dell'ID di generazione macchina virtuale salvato nella macchina virtuale perché corrisponda al nuovo valore (G2) archiviato nell'hypervisor. Le misure di sicurezza fanno sì che la replica converga per entrambi i controller di dominio.

Con Windows Server 2012, Active Directory Domain Services utilizza misure di sicurezza nei controller di dominio virtuali ospitati in hypervisor compatibili con VM-GenerationID e fa sì che l'applicazione accidentale di snapshot o altri sistemi analoghi abilitati per hypervisor che potrebbero eseguire il rollback dello stato di una macchina virtuale non determinino l'interruzione dell'ambiente Active Directory Domain Services, prevenendo eventuali problemi di replica, ad esempio relativi agli USN o a oggetti residui.

Il ripristino di un controller di dominio tramite l'applicazione di uno snapshot della macchina virtuale non è consigliato come meccanismo alternativo per il backup di un controller di dominio. È consigliabile continuare a utilizzare Windows Server Backup o altre soluzioni di backup basate sul writer del servizio Copia Shadow.

> [!CAUTION]
> Se un controller di dominio in un ambiente di produzione viene accidentalmente ripristinato a uno snapshot, è consigliabile che rivolgersi ai fornitori per le applicazioni e servizi ospitati su tale macchina virtuale, per istruzioni su come verificare lo stato di tali programmi dopo snapshot ripristino.

Per ulteriori informazioni, vedere [architettura di ripristino sicuro di controller di dominio virtualizzati](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="recovering-from-a-usn-rollback"></a>Ripristino da un rollback degli USN

Esistono due approcci per eseguire il ripristino da un rollback degli USN:

* Rimuovere il controller di dominio dal dominio
* Ripristinare lo stato del sistema di un backup valido

### <a name="remove-the-domain-controller-from-the-domain"></a>Rimuovere il controller di dominio dal dominio

1. Rimuovi Active Directory dal controller di dominio per forzarlo come server autonomo.
2. Arresta il server abbassato di livello.
3. In un controller di dominio integro pulisci i metadati del controller di dominio abbassato di livello.
4. Se il controller di dominio ripristinato in modo errato ospita i ruoli di master operazioni, trasferisci questi ruoli in un controller di dominio integro.
5. Riavvia il server abbassato di livello.
6. Se necessario, installa di nuovo Active Directory nel server autonomo.
7. Se in precedenza il controller di dominio era un catalogo globale, configuralo come catalogo globale.
8. Se in precedenza il controller di dominio ospitava ruoli di master operazioni, trasferisci di nuovo tali ruoli nel controller di dominio.

### <a name="restore-the-system-state-of-a-good-backup"></a>Ripristinare lo stato del sistema di un backup valido

Valuta se esistono backup dello stato del sistema validi per questo controller di dominio. Se è stato eseguito un backup dello stato del sistema valido prima del ripristino errato del controller di dominio di cui è stato eseguito il rollback e il backup contiene modifiche recenti apportate al controller di dominio, ripristina lo stato del sistema dal backup più recente.

Come origine di un backup puoi anche usare lo snapshot. In alternativa, puoi configurare il database in modo che assegni a se stesso un nuovo ID chiamata seguendo la procedura illustrata nella sezione [Ripristinare un controller di dominio virtuale quando non è disponibile un backup appropriato dei dati di stato del sistema](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available).

## <a name="next-steps"></a>Passaggi successivi

* Per ulteriori informazioni sulla risoluzione dei problemi, vedere l'[articolo sulla risoluzione dei problemi relativi ai controller di dominio virtualizzati](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).
* [Informazioni dettagliate sul servizio Ora di Windows (W32Time)](../../networking/windows-time-service/windows-time-service-top.md)
