---
title: Safely virtualizing Active Directory Domain Services (AD DS)
description: Rollback degli USN e virtualizzazione sicura di Active Directory
ms.topic: article
ms.prod: windows-server-threshold
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: aa84e09e8a958193fee82c7b9c03cd1dca910c55
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/24/2019
ms.locfileid: "63684173"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>Safely virtualizing Active Directory Domain Services (AD DS)

>Si applica a: Windows Server

A partire da Windows Server 2012, Active Directory Domain Services offre maggiore supporto per la virtualizzazione dei controller di dominio grazie all'introduzione di funzionalità di virtualizzazione. Questo articolo illustra il ruolo di numeri USN e InvocationIDs nella replica del Controller di dominio e alcuni potenziali problemi che possono verificarsi.

## <a name="update-sequence-number-and-invocationid"></a>Numero di sequenza di aggiornamento e InvocationID

Gli ambienti virtualizzati pongono sfide straordinarie per i carichi di lavoro distribuiti che dipendono da uno schema di replica basato su clock logico. La replica di Servizi di dominio Active Directory, ad esempio, utilizza un valore con incremento monotono, denominato numero di sequenza di aggiornamento o USN, assegnato alle transazioni in ogni controller di dominio. Istanza del ogni controller di dominio database viene inoltre assegnata un'identità, denominata ID chiamata. L'ID chiamata di un controller di dominio e il relativo USN fungono insieme da identificatore univoco associato a ogni transazione di scrittura eseguita in ogni controller di dominio e devono essere univoci all'interno della foresta.

La replica di Servizi di dominio Active Directory utilizza l'ID chiamata e gli USN in ogni controller di dominio per determinare le modifiche che è necessario replicare in altri controller di dominio. Se un controller di dominio verrà riportato nel tempo all'esterno di presenza del controller di dominio e viene riutilizzato un USN per una transazione interamente diversa, la replica non convergerà perché altri controller di dominio verrà ritiene di che avere già ricevuto l'aggiornamento associato all'USN riutilizzato nel contesto di tale ID.

Nell'illustrazione seguente, ad esempio, viene mostrata la sequenza di eventi che si verifica in Windows Server 2008 R2 e nei sistemi operativi precedenti quando il rollback degli USN viene rilevato in VDC2, ovvero il controller di dominio di destinazione eseguito in una macchina virtuale. In questa illustrazione, il rilevamento del rollback degli USN si verifica su VDC2 quando un partner di replica rileva che VDC2 ha inviato un valore USN di aggiornamento che è stato individuato in precedenza dal partner di replica, a indicare che il database di VDC2 ha rollback nel tempo in modo non corretto.

![La sequenza di eventi quando viene rilevato rollback degli USN](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Una macchina virtuale (VM) semplifica per gli amministratori degli hypervisor ripristinare un dominio USN del controller (il clock logico), ad esempio, l'applicazione di uno snapshot di fuori di presenza del controller di dominio. Per ulteriori informazioni sugli USN e USN rollback, inclusa un'altra illustrazione per illustrare le istanze non rilevate del rollback degli USN, vedere [USN e Rollback degli USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

A partire da Windows Server 2012, il controller di dominio virtuali AD DS ospitato in piattaforme hypervisor che espongono un identificatore denominato ID di generazione VM può rilevare e utilizzare misure di sicurezza necessarie per proteggere l'ambiente di dominio Active Directory se la macchina virtuale è rollback in tempo per l'applicazione di uno snapshot della macchina Virtuale. Questa progettazione utilizza un meccanismo indipendente dal fornitore dell'hypervisor per esporre l'identificatore nello spazio degli indirizzi della macchina virtuale guest, in modo da garantire un'esperienza di virtualizzazione sicura in qualsiasi hypervisor che supporta tale ID di generazione macchina virtuale. Questo identificatore può essere campionato da servizi e applicazioni eseguiti all'interno della macchina virtuale per rilevare se è stato eseguito il rollback di una macchina virtuale in tempo.

## <a name="effects-of-usn-rollback"></a>Effetti del rollback degli USN

Quando si verificano i rollback degli USN, le modifiche a oggetti e attributi non sono in ingresso replicata dal controller di dominio di destinazione che hanno visualizzato in precedenza l'USN.

Poiché questi controller di dominio di destinazione ritengono di essere aggiornati, errori di replica non vengono segnalati nei log eventi del servizio Directory o dagli strumenti di monitoraggio e diagnostici.

Rollback degli USN può influire sulla replica di qualsiasi oggetto o attributo in qualsiasi partizione. L'effetto collaterale più spesso osservato è che gli account utente e account del computer che vengono creati nel controller di dominio di rollback non esistono in uno o più partner di replica. In alternativa, gli aggiornamenti delle password originati nel controller di dominio di rollback non esistono per i partner di replica.

Un rollback degli USN può impedire la replica di qualsiasi tipo di oggetto in qualsiasi partizione di Active Directory. I tipi di oggetti seguenti:

* La topologia di replica di Active Directory e pianificazione
* La presenza di controller di dominio nella foresta e i ruoli che contengono questi controller di dominio
* L'esistenza di partizioni dell'applicazione e di dominio nella foresta
* L'esistenza di gruppi di sicurezza e l'appartenenza al gruppo corrente
* Registrazione dei record DNS nelle zone DNS integrate in Active Directory

La dimensione del foro USN può rappresentare centinaia, migliaia o persino decine di migliaia di modifiche a utenti, computer, i trust, le password e gruppi di sicurezza. Il problema di USN viene definito dalla differenza tra il numero più alto USN che esistevano quando è stato effettuato il backup dello stato del sistema ripristinato e modificato il numero di origine che sono stati creati nel controller di dominio di rollback, prima è stato portato offline.

## <a name="detecting-a-usn-rollback"></a>Rilevamento di un rollback degli USN

Poiché è difficile rilevare un rollback degli USN, un controller di dominio registra l'evento 2095 quando un controller di dominio di origine invia un numero USN riconosciuto in precedenza a un controller di dominio di destinazione senza una corrispondente variazione l'ID di chiamata.

Per evitare univoci che hanno origine gli aggiornamenti per Active Directory di creare nel controller di dominio ripristinato in modo non corretto, il servizio Accesso rete è in pausa. Quando il servizio Accesso rete è in pausa, account utente e computer non è possibile modificare la password in un controller di dominio che verrà non in uscita-replica tali modifiche. Analogamente, gli strumenti di amministrazione di Active Directory verranno favorito un controller di dominio integro quando effettuano aggiornamenti agli oggetti in Active Directory.

In un controller di dominio, i messaggi di evento simili ai seguenti vengono registrati se vengono soddisfatte le condizioni seguenti:

* Un controller di dominio di origine invia un numero USN riconosciuto in precedenza a un controller di dominio di destinazione.
* Sussiste alcuna modifica corrispondente nell'ID chiamata.

Questi eventi possono essere acquisiti nel registro eventi del servizio Directory. Tuttavia, possono essere sovrascritti prima che vengono lette dall'amministratore.

Se si sospetta che un rollback degli USN si è verificato ma non viene visualizzato un evento corrispondente nel caso in cui i log, cercare la voce del Registro di sistema DSA non scrivibile. Questa voce fornisce prove che si è verificato un rollback degli USN.

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> L'eliminazione o modifica manualmente il valore della voce del Registro di sistema Dsa non scrivibile inserisce il controller di dominio di rollback in uno stato non supportato in modo permanente. Pertanto, tali modifiche non sono supportate. In particolare, la modifica del valore rimuove il comportamento di quarantena aggiunto per il codice di rilevamento del rollback degli USN. Le partizioni di Active Directory nel controller di dominio di rollback sarà incoerente in modo permanente con i partner di replica dirette e transitive nella stessa foresta Active Directory.

Altre informazioni su questa passaggi chiave e la risoluzione del Registro di sistema sono reperibili nell'articolo del supporto tecnico [Active Directory replica errore 8456 o 8457: "L'origine | server di destinazione rifiuta le richieste di replica"](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination).

## <a name="virtualization-based-safeguards"></a>Misure di sicurezza basato su virtualizzazione

Durante l'installazione di controller di dominio Active Directory archivia inizialmente l'identificatore di generazione macchina Virtuale come parte dell'attributo msDS-GenerationID sull'oggetto computer del controller di dominio nel proprio database (noto anche come la directory information tree o DIT). L'ID di generazione macchina virtuale viene registrato in modo indipendente da un driver Windows all'interno della macchina virtuale.

Quando un amministratore ripristina la macchina virtuale da uno snapshot precedente, il valore corrente dell'ID di generazione macchina virtuale ottenuto dal driver della macchina virtuale viene confrontato con un valore nel nell'albero delle informazioni di directory.

Se i due valori sono diversi, l'ID chiamata viene reimpostato e il pool di RID viene ignorato impedendo in tal modo il riutilizzo dell'USN. Se i valori sono identici, viene eseguito il commit della transazione come normale.

Servizi di dominio Active Directory confronta inoltre il valore corrente dell'ID di generazione macchina virtuale della macchina virtuale con il valore nell'albero delle informazioni di directory ogni volta che viene riavviato il controller di dominio e, se diverso, reimposta l'ID chiamata, ignora il pool di RID e aggiorna l'albero delle informazioni di directory con il nuovo valore. Sincronizza inoltre in modo non autorevole la cartella SYSVOL per completare il ripristino sicuro. In questo modo le misure di sicurezza possono estendersi all'applicazione di snapshot nelle macchine virtuali arrestate. Queste misure di sicurezza introdotti in Windows Server 2012 consentono agli amministratori di dominio Active Directory beneficiare dei vantaggi univoci di distribuzione e gestione di controller di dominio in un ambiente virtualizzato.

La figura seguente mostra come le misure di sicurezza vengono applicate quando lo stesso rollback degli USN viene rilevato in un controller di dominio virtualizzato che esegue Windows Server 2012 in un hypervisor che supporta VM-GenerationID.

![Misure di sicurezza applicati quando viene rilevato lo stesso rollback degli USN](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

In questo caso, quando l'hypervisor rileva una modifica al valore dell'ID di generazione macchina virtuale, vengono attivate misure di sicurezza di virtualizzazione, inclusi la reimpostazione dell'ID chiamata per il controller di dominio virtualizzato (da A a B nell'esempio precedente) e l'aggiornamento del valore dell'ID di generazione macchina virtuale salvato nella macchina virtuale perché corrisponda al nuovo valore (G2) archiviato nell'hypervisor. Le misure di sicurezza fanno sì che la replica converga per entrambi i controller di dominio.

Con Windows Server 2012, Active Directory Domain Services si avvale di misure di sicurezza nei controller di dominio virtuali ospitati in hypervisor compatibile con VM-GenerationID e assicura che l'applicazione accidentale di snapshot o altri sistemi analoghi di abilitati per hypervisor che potrebbero eseguire il rollback virtuale stato della macchina non interferire con l'ambiente Active Directory Domain Services (per evitare problemi di replica, ad esempio una bolla USN o oggetti residui).

Il ripristino di un controller di dominio applicando uno snapshot macchina virtuale non è consigliato come meccanismo alternativo per il backup di un controller di dominio. È consigliabile continuare a utilizzare Windows Server Backup o altre soluzioni di backup basate sul writer del servizio Copia Shadow.

> [!CAUTION]
> Se un controller di dominio in un ambiente di produzione viene accidentalmente ripristinato a uno snapshot, è consigliabile che rivolgersi ai fornitori per le applicazioni e servizi ospitati su tale macchina virtuale, per istruzioni su come verificare lo stato di tali programmi dopo snapshot ripristino.

Per ulteriori informazioni, vedere [architettura di ripristino sicuro di controller di dominio virtualizzati](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="recovering-from-a-usn-rollback"></a>Il ripristino da un rollback degli USN

Esistono due approcci per il ripristino da un rollback degli USN:

* Rimuovere il Controller di dominio dal dominio
* Ripristinare lo stato del sistema di un backup valido

### <a name="remove-the-domain-controller-from-the-domain"></a>Rimuovere il Controller di dominio dal dominio

1. Rimuovere Active Directory dal controller di dominio per forzarlo a diventare un server autonomo.
2. Arrestare il server abbassato di livello.
3. In un controller di dominio integro pulire i metadati del controller di dominio abbassato di livello.
4. Se il dominio non correttamente ripristinato controller ospita ruoli master operazioni, trasferire questi ruoli a un controller di dominio integro.
5. Riavviare il server abbassato di livello.
6. Se è necessario, installare Active Directory nuovamente nel server autonomo.
7. Se il controller di dominio era in precedenza un catalogo globale, configurare il controller di dominio da un catalogo globale.
8. Se il controller di dominio ospitato in precedenza ruoli master operazioni, trasferire tali ruoli di nuovo al controller di dominio.

### <a name="restore-the-system-state-of-a-good-backup"></a>Ripristinare lo stato del sistema di un backup valido

Valutare se esistono backup dello stato del sistema valido per questo controller di dominio. Se è stato eseguito un backup dello stato del sistema valido prima che il controller di dominio di rollback è stato ripristinato in modo non corretto e il backup contiene modifiche recenti apportate nel controller di dominio, ripristinare lo stato del sistema dal backup più recente.

È anche possibile usare lo snapshot come origine di un backup. Oppure è possibile impostare il database per concedere a se stesso un nuovo ID chiamata seguendo la procedura nella sezione [ripristino di un controller di dominio virtuale quando non è disponibile un backup dei dati dello stato del sistema appropriato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)

## <a name="next-steps"></a>Passaggi successivi

* Per ulteriori informazioni sulla risoluzione dei problemi, vedere l'[articolo sulla risoluzione dei problemi relativi ai controller di dominio virtualizzati](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).
* [Informazioni dettagliate sul servizio ora di Windows (W32Time)](../../networking/windows-time-service/windows-time-service-top.md)
