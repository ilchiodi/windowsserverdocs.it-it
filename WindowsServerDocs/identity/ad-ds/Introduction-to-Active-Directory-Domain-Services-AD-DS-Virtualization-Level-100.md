---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: Introduzione alla virtualizzazione (AD DS) di servizi di dominio Active Directory (livello 100)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3c7fdc2e20bc4a27f2555af54f396a15f664d6c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>Introduzione alla virtualizzazione (AD DS) di servizi di dominio Active Directory (livello 100)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La virtualizzazione di ambienti servizi di dominio Active Directory (AD DS) è stato in corso per un numero di anni. A partire da Windows Server 2012, servizi di dominio Active Directory offre maggiore supporto per la virtualizzazione dei controller di dominio grazie all'introduzione di funzionalità di virtualizzazione e alla rapida distribuzione dei controller di dominio virtuali tramite clonazione. Queste nuove funzionalità di virtualizzazione offrono maggiore supporto per cloud pubblici e privati, ambienti ibridi in cui parti di dominio Active Directory esistono in locale e nel cloud e infrastrutture di dominio Active Directory che risiedono completamente in locale.

**In questo documento**

-   [Virtualizzazione sicura dei controller di dominio](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#safe_virt_dc)

-   [Clonazione del controller di dominio virtualizzati](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#virtualized_dc_cloning)

-   [Passaggi per la distribuzione di un controller di dominio virtualizzato clone](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc)

-   [Risoluzione dei problemi](../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#troubleshooting)

## <a name="safe_virt_dc"></a>Virtualizzazione sicura dei controller di dominio
Gli ambienti virtualizzati pongono sfide straordinarie per i carichi di lavoro distribuiti che dipendono da una combinazione di replica basato su clock logico. Replica di Active Directory, ad esempio, Usa un valore di incremento monotono, denominato (noto come un numero di sequenza di aggiornamento o USN) assegnato alle transazioni in ogni controller di dominio. Istanza del database del ogni controller di dominio viene inoltre assegnata un'identità, denominata ID chiamata. L'ID chiamata di un controller di dominio e il relativo USN fungono insieme da identificatore univoco associato a ogni transazione di scrittura eseguita su ogni controller di dominio e deve essere univoco all'interno della foresta.

La replica di Active Directory utilizza InvocationID e gli USN in ogni controller di dominio per determinare quali modifiche devono essere replicate in altri controller di dominio. Se un controller di dominio viene eseguito il rollback nel tempo all'esterno di presenza del controller di dominio e viene riutilizzato un USN per una transazione interamente diversa, la replica non convergerà perché altri controller di dominio riterranno di che avere già ricevuto l'aggiornamento associato all'USN riutilizzato nel contesto dell'ID chiamata.

Ad esempio, nella figura seguente mostra la sequenza di eventi che si verifica in Windows Server 2008 R2 e sistemi operativi precedenti quando il rollback degli USN viene rilevato in vdc2, ovvero il controller di dominio di destinazione in cui è in esecuzione in una macchina virtuale. In questa illustrazione, il rilevamento del rollback degli USN si verifica su VDC2 quando un partner di replica rileva che VDC2 ha inviato un valore USN di aggiornamento che è stato individuato in precedenza dal partner di replica, che indica che il database di VDC2 ha eseguito il rollback in tempo in modo errato.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Una macchina virtuale (VM) semplifica per gli amministratori degli hypervisor eseguire il rollback di un dominio USN del controller (il clock logico), ad esempio, l'applicazione di uno snapshot di fuori di presenza del controller di dominio. Per ulteriori informazioni sugli USN e USN rollback, inclusa un'altra illustrazione in cui vengono mostrate le istanze non rilevate del rollback degli USN, vedere [USN e Rollback degli USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

A partire da Windows Server 2012, il controller di dominio virtuali AD DS ospitato in piattaforme hypervisor che espongono un identificatore denominato ID VM-Generation grado di rilevare e utilizzare misure di sicurezza necessarie per proteggere l'ambiente di dominio Active Directory se la macchina virtuale viene eseguito il rollback in tempo per l'applicazione di uno snapshot della macchina virtuale. La progettazione VM-GenerationID utilizza un meccanismo indipendente dal fornitore dell'hypervisor per esporre l'identificatore nello spazio degli indirizzi della macchina virtuale guest, pertanto è disponibile in modo coerente di qualsiasi hypervisor che supporta VM-GenerationID l'esperienza di virtualizzazione sicura. Questo identificatore può essere campionato da servizi e applicazioni in esecuzione all'interno della macchina virtuale per rilevare se una macchina virtuale è stato eseguito il rollback in tempo.

### <a name="BKMK_HowSafeguardsWork"></a>Come funzionano queste misure di sicurezza della virtualizzazione?
Durante l'installazione, controller di dominio Active Directory archivia inizialmente l'identificatore di generazione macchina virtuale come parte dell'attributo msDS-GenerationID sull'oggetto computer del controller di dominio nel proprio database (noto anche come la directory information tree o DIT). L'ID di generazione VM viene registrato in modo indipendente da un driver di Windows all'interno della macchina virtuale.

Quando un amministratore Ripristina la macchina virtuale da uno snapshot precedente, il valore corrente dell'ID di generazione del driver della macchina virtuale viene confrontato con un valore nel DIT.

Se i due valori sono diversi, l'ID chiamata viene reimpostato e il pool di RID scartati impedendo in tal modo riutilizzo di numeri USN. Se i valori sono identici, la transazione viene eseguito il commit come di consueto.

Servizi di dominio Active Directory confronta inoltre il valore corrente dell'ID di generazione della macchina virtuale in base al valore DIT ogni volta che il controller di dominio viene riavviato e, se diverso, che reimposta l'ID di chiamata, rimuove il pool di RID e aggiorna il DIT con il nuovo valore. Inoltre non autorevole, si sincronizza la cartella SYSVOL per completare il ripristino sicuro. In questo modo le misure di sicurezza possono estendersi all'applicazione di snapshot nelle macchine virtuali arrestate. Queste misure di sicurezza introdotti in Windows Server 2012 consentono agli amministratori di dominio Active Directory di beneficiare dei vantaggi univoci di distribuzione e gestione di controller di dominio in un ambiente virtualizzato.

La figura seguente mostra come misure di sicurezza vengono applicate quando viene rilevato il rollback degli USN stesso in un controller di dominio virtualizzato che esegue Windows Server 2012 in un hypervisor che supporta VM-GenerationID.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

In questo caso, quando l'hypervisor rileva una modifica al valore VM-GenerationID, vengono attivate misure di sicurezza della virtualizzazione, inclusi la reimpostazione dell'ID chiamata per il controller di dominio virtualizzato (da A B nell'esempio precedente) e l'aggiornamento del valore VM-GenerationID salvato nella macchina virtuale in modo che corrisponda il nuovo valore (G2) archiviato nell'hypervisor. Le misure di sicurezza assicurarsi che la replica converga per entrambi i controller di dominio.

Con Windows Server 2012, servizi di dominio Active Directory utilizza misure di sicurezza nei controller di dominio virtuali ospitati in hypervisor compatibile con VM-GenerationID e garantisce che l'applicazione accidentale di snapshot o altri sistemi analoghi di abilitati per hypervisor che potrebbero eseguire il rollback dello stato di una macchina virtuale non interruzione con l'ambiente di dominio Active Directory (per evitare problemi di replica, ad esempio una bolla USN o oggetti residui). Tuttavia, il ripristino di un controller di dominio tramite l'applicazione di uno snapshot macchina virtuale non è consigliato come meccanismo alternativo per eseguire il backup di un controller di dominio. È consigliabile continuare a utilizzare Windows Server Backup o altre soluzioni di backup basate sul writer VSS.

> [!CAUTION]
> Se un controller di dominio in un ambiente di produzione viene accidentalmente ripristinato a uno snapshot, è consigliabile che rivolgersi ai fornitori per le applicazioni e servizi ospitati su tale macchina virtuale, per istruzioni su come verificare lo stato di tali programmi dopo snapshot ripristino.

Per ulteriori informazioni, vedere [architettura di ripristino sicuro di controller di dominio virtualizzati](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="virtualized_dc_cloning"></a>Clonazione del controller di dominio virtualizzati
A partire da Windows Server 2012, gli amministratori possono facilmente e in modo sicuro distribuire controller di dominio di replica copiando un controller di dominio virtuale esistente. In un ambiente virtuale, gli amministratori non è più necessario ripetutamente distribuire un'immagine server preparata utilizzando sysprep.exe, promuovere il server a un controller di dominio e quindi completare requisiti di configurazione aggiuntivi per la distribuzione di ogni controller di dominio di replica.

> [!NOTE]
> Gli amministratori devono seguire processi esistenti per distribuire il primo controller di dominio in un dominio, ad esempio utilizzare sysprep.exe per preparare un disco rigido server virtuale (VHD), promuovere il server a un controller di dominio e quindi completare eventuali requisiti di configurazione aggiuntivi. In uno scenario di ripristino di emergenza, utilizzare il backup del server più recente per ripristinare il primo controller di dominio in un dominio.

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>Scenari di beneficiano di clonazione del controller di dominio virtuale

-   Distribuzione rapida di controller di dominio aggiuntivo in un nuovo dominio

-   Ripristinare rapidamente la continuità aziendale durante il ripristino di emergenza ripristinando la capacità di dominio Active Directory tramite una rapida distribuzione dei controller di dominio con la clonazione

-   Ottimizzare le distribuzioni di cloud privato da provisioning elastico dei controller di dominio per soddisfare i requisiti di scala maggiore

-   Il provisioning rapido degli ambienti di test consente la distribuzione e test di nuove caratteristiche e funzionalità prima dell'implementazione di produzione

-   Esigenze rapidamente maggiore capacità nelle succursali tramite la clonazione del controller di dominio esistenti nelle succursali

Quando si distribuisce rapidamente un numero elevato di controller di dominio, continuare a eseguire le procedure esistenti per convalidare l'integrità di ogni controller di dominio al termine dell'installazione. Distribuire controller di dominio in batch di dimensioni ragionevoli in modo che è possibile convalidare l'integrità dopo ogni batch di installazioni è stata completata. La dimensione batch consigliata è 10. Per ulteriori informazioni, vedere [passaggi per la distribuzione di un controller di dominio virtualizzato clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc).

### <a name="clear-separation-of-responsibilities"></a>Netta separazione delle responsabilità
L'autorizzazione a clonare controller di dominio virtualizzati è controllata dall'amministratore di dominio Active Directory. Affinché gli amministratori degli hypervisor distribuire controller di dominio aggiuntivi copiando controller di dominio virtuali, l'amministratore di dominio Active Directory deve selezionare e autorizzare un controller di dominio e quindi eseguire i passaggi preparatori per abilitarlo come origine per la clonazione.

Con il provisioning delle macchine virtuali in genere sotto il provisioning dell'amministratore dell'hypervisor, gli amministratori degli hypervisor possono eseguire il provisioning di macchine virtuali controller di dominio replica copiando controller di dominio virtualizzati che siano stati autorizzati e preparati per la clonazione dall'amministratore di dominio Active Directory.

> [!WARNING]
> Chiunque sia autorizzato ad amministrare l'hypervisor che ospita un controller di dominio virtuale deve essere altamente attendibile e controllato nell'ambiente.

### <a name="how-does-virtual-domain-controller-cloning-work"></a>Come la clonazione del controller di dominio virtuale?
Il processo di clonazione implica la copia del disco rigido virtuale del controller di dominio virtuale un esistente (oppure, per le configurazioni più complesse, la macchina virtuale del controller di dominio), autorizzare per la clonazione di dominio Active Directory e la creazione di un file di configurazione clone. Ciò riduce il numero di passaggi e il tempo necessari per la distribuzione di un controller di dominio virtuale di replica, in caso contrario eliminando le attività di distribuzione ripetitive.

Il controller di dominio clone utilizza i criteri seguenti per rilevare che si tratta di una copia di un altro controller di dominio:

1.  Il valore dell'ID VM-Generation fornito dalla macchina virtuale è diverso dal valore dell'ID VM-Generation archiviati nel DIT.

    > [!NOTE]
    > La piattaforma hypervisor deve supportare l'ID VM-Generation (Windows Server 2012 Hyper-V supporta l'ID VM-Generation).

2.  Presenza di un file denominato DCCloneConfig.xml in uno dei seguenti percorsi:

    -   La directory in cui risiede il DIT

    -   %windir%\NTDS

    -   La radice di un'unità rimovibile

Una volta soddisfatti i criteri, passano attraverso il processo di clonazione effettuare il provisioning di se stesso come controller di dominio di replica.

Il controller di dominio clone utilizza il contesto di sicurezza del controller di dominio di origine (il cui rappresenta una copia di controller di dominio) di contattare il titolare di ruolo di master di Windows Server 2012 Domain Controller primario (PDC) emulatore operazioni (noto anche come flexible single master operation o FSMO). L'emulatore PDC deve essere in esecuzione Windows Server 2012, ma non deve essere in esecuzione in un hypervisor.

> [!NOTE]
> Se si dispone di un'estensione dello schema con attributi che fanno riferimento a controller di dominio e l'attributo è uno degli oggetti copiati (oggetto computer, oggetto impostazioni NTDS) per creare il clone, tale attributo non verrà copiato o aggiornato per fare riferimento il controller di dominio clone.

Dopo aver verificato che il controller di dominio richiedente è autorizzato per la clonazione, l'emulatore PDC crea una nuova identità macchina includendo un nuovo account, SID, nome e la password che identifichino la macchina come controller di dominio di replica e inviare informazioni al clone. Il controller di dominio clone prepara quindi i file di database di dominio Active Directory come una replica e esegue la pulizia dello stato del computer.

Per ulteriori informazioni, vedere [architettura di clonazione del controller di dominio virtualizzati](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch).

### <a name="cloning-components"></a>Componenti di clonazione
I componenti di clonazione includono nuovi cmdlet nel modulo Active Directory per Windows PowerShell e i file XML associati:

-   **New-ADDCCloneConfigFile** "questo cmdlet crea e inserisce DCCloneConfig.xml nella posizione corretta per assicurarsi che sia disponibile per attivare la clonazione. Esegue inoltre controlli dei prerequisiti per garantire la corretta clonazione. È inclusa nel modulo Active Directory per Windows PowerShell. È possibile eseguirlo in locale in un controller di dominio virtualizzato preparato per la clonazione oppure è possibile eseguire in remoto utilizzando l'opzione - offline. È possibile specificare le impostazioni per il controller di dominio clone, ad esempio il nome del sito e l'indirizzo IP.

    I controlli dei prerequisiti eseguiti sono:

    > [!NOTE]
    > I controlli dei prerequisiti non vengono eseguite quando la "opzione offline è utilizzato. Per ulteriori informazioni, vedere [esecuzione New-ADDCCloneConfigFile in modalità offline](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).

    -   Il controller di dominio preparato è autorizzato per la clonazione (è un membro del **controller di dominio clonabili** gruppo)

    -   L'emulatore PDC esegue Windows Server 2012.

    -   Tutti i programmi o servizi elencati esecuzione **Get-ADDCCloningExcludedApplicationList** sono inclusi in CustomDCCloneAllowList.xml (descritto in modo più dettagliato alla fine dell'elenco dei componenti di clonazione).

-   **DCCloneConfig.xml** "per clonare correttamente un controller di dominio virtualizzati, questo file deve essere presente nella directory in cui risiede il DIT, *%windir%\NTDS*, o la radice di un'unità rimovibile. Oltre a essere utilizzato come uno dei trigger per rilevare e avviare la clonazione, fornisce inoltre un modo per specificare le impostazioni di configurazione per il controller di dominio clone.

    Lo schema e un file di esempio per il file DCCloneConfig.xml vengono archiviati in tutti i computer Windows Server 2012 in:

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.Xml

    È consigliabile che usare il cmdlet New-ADDCCloneConfigFile per creare il file DCCloneConfig.xml. Benché sia possibile usare anche il file di schema con un editor XML compatibile con per creare questo file, modifica manuale del file aumenta la probabilità di errori. Se si modifica il file, deve essere eseguita utilizzando un editor XML, ad esempio Visual Studio, [XML Notepad](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973), o applicazioni di terze parti (non usare il blocco note).

-   **Get-ADDCCloningExcludedApplicationList** "questo cmdlet viene eseguito sul controller di dominio di origine prima di iniziare il processo di clonazione per determinare quali servizi o programmi installati non presente nell'elenco supportati predefinito, DefaultDCCloneAllowList.xml, o un'inclusione definito dall'utente elenco denominato file CustomDCCloneAllowList.xml e pertanto non valutati per l'impatto della clonazione.

    Questo cmdlet Cerca il controller di dominio di origine per i servizi in Gestione controllo servizi e programmi installati elencati in **HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall** che non sono specificati nel (DefaultDCCloneAllowList.xml) elenco predefinito o, se disponibile, l'elenco di inclusione definito dall'utente (CustomDCCloneAllowList.xml file). L'elenco di applicazioni e servizi restituito eseguendo il cmdlet è la differenza tra quello già specificato nel DefaultDCCloneAllowList.xml o CustomDCCloneAllowList.xml file e l'elenco creato in fase di esecuzione, in base a ciò che è installato nel controller di dominio di origine. L'output di servizi e programmi di Get-ADDCCloningExcludedApplicationList può essere aggiunti al file CustomDCCloneAllowList.xml se si stabilisce che i servizi e i programmi possono essere clonati in modo sicuro. Per determinare se un servizio o un programma installato può essere clonato in modo sicuro, valutare le condizioni seguenti:

    -   È il servizio o programma installato interessati dall'identità macchina, ad esempio nome, SID, password e così via?

    -   Il servizio o programma installato archivia qualsiasi stato localmente nel computer che possono interessarne la funzionalità nel clone?

    È necessario collaborare con il fornitore del software dell'applicazione per determinare se il servizio o programma può essere clonato in modo sicuro.

    > [!NOTE]
    > Prima di provisioning di altri servizi o programmi nel file CustomDCCloneAllowList.xml, verificare se si dispone della licenza necessaria per copiare il software contenuto su tale macchina virtuale.

    Se le applicazioni non sono clonabili, rimuoverle dal controller di dominio di origine prima di creare il supporto clone. Se un'applicazione viene visualizzato nell'output del cmdlet, ma non è incluso nel file CustomDCCloneAllowList.xml, la clonazione non riuscirà. Per la clonazione riesca, l'output del cmdlet non deve includere tutti i servizi o programmi. In altre parole, un'applicazione deve essere incluso nel file CustomDCCloneAllowList.xml o rimossi dal controller di dominio di origine.

    La tabella seguente illustra le opzioni per l'esecuzione di Get-ADDCCloningExcludedApplicationList.

    |||
    |-|-|
    |Argomento|Spiegazione|
    |*<no argument specified>*|Visualizza un elenco di servizi o programmi nella console che non sono stati considerati per la clonazione. Se è già presente un CustomDCCloneAllowList.XML in una delle posizioni consentite, usa tale file per visualizzare il rimanente servizi e programmi (che possono essere zero se gli elenchi corrispondono).|
    |-GenerateXml|Crea il file CustomDCCloneAllowList.XML popolato con i servizi e i programmi elencati nella console.|
    |-Force|Sovrascrive un file CustomDCCloneAllowList.XML esistente.|
    |-Path|Percorso di cartella per creare CustomDCCloneAllowList.XML.|

-   **DefaultDCCloneAllowList.xml** "questo file è presente per impostazione predefinita in ogni Windows Server 2012 domain controller nel *%windir%\system32*. Elenca i servizi e i programmi installati che possono essere clonati in modo sicuro per impostazione predefinita. Non è possibile modificare il percorso o il contenuto di questo file o la clonazione non riuscirà.

-   **CustomDCCloneAllowList.xml** "Se si dispone di servizi o programmi installati che si trovano nel controller di dominio di origine non compresi tra quelli elencati nel file DefaultDCCloneAllowList.xml, tali programmi e servizi devono essere incluso in questo file. Per trovare i servizi o programmi installati non elencati nel nel file DefaultDCCloneAllowList.xml, eseguire il **Get-ADDCCloningExcludedApplicationList** cmdlet. È consigliabile utilizzare il **"GenerateXml** argomento per generare il file XML.

    Il processo di clonazione controllate le posizioni seguenti nell'ordine per questo file e utilizza il primo file XML trovato, indipendentemente dal contenuto della cartella:

    1.  La seguente chiave del Registro di sistema:

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  Directory di lavoro DSA

    3.  %systemroot%\ntds

    4.  Supporto rimovibile di lettura/scrittura, in ordine di lettera di unità, alla radice dell'unità

### <a name="deployment-scenarios"></a>Scenari di distribuzione
Scenari di distribuzione seguenti sono supportati per la clonazione di controller di dominio virtuale:

-   Distribuire un controller di dominio clone eseguendo una copia del file di disco rigido virtuale (vhd) del controller di dominio di origine.

-   Distribuire un controller di dominio clone copiando la macchina virtuale di un controller di dominio di origine utilizzando la semantica di esportazione/importazione esposta dall'hypervisor.

> [!NOTE]
> I passaggi nella sezione [passaggi per la distribuzione di un controller di dominio virtualizzato clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc) viene descritta la copia di una macchina virtuale utilizzando la funzionalità di esportazione/importazione di Windows Server 2012 Hyper-V.

## <a name="steps_deploy_vdc"></a>Passaggi per la distribuzione di un controller di dominio virtualizzato clone

-   [Prerequisiti](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [Passaggio 1: Il controller di dominio virtualizzato l'autorizzazione per la clonazione](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [Passaggio 2: Esegui Get-ADDCCloningExcludedApplicationList cmdlet](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [Passaggio 3: Eseguire New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [Passaggio 4: Esportare e quindi importare la macchina virtuale del controller di dominio di origine](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>Prerequisiti

-   Per completare i passaggi nelle procedure seguenti, è necessario essere un membro del gruppo Domain Admins o dispongano di autorizzazioni equivalenti assegnate.

-   Da un prompt dei comandi con privilegi elevati, è necessario eseguire i comandi di Windows PowerShell utilizzati in questa Guida. A tale scopo, fare clic il **Windows PowerShell** icona, quindi fare clic su **Esegui come amministratore**.

-   Un server Windows Server 2012 con il ruolo server Hyper-V installato (**HyperV1**).

-   Un secondo server di Windows Server 2012 con il ruolo server Hyper-V installato (**HyperV2**).

    > [!NOTE]
    > -   Se si utilizza un altro hypervisor, è consigliabile rivolgersi al fornitore dell'hypervisor che per verificare se l'hypervisor supporti l'ID di VM-Generation. Se l'hypervisor non supporta l'ID VM-Generation ed è stato fornito un DCCloneConfig.xml, la nuova macchina virtuale verrà avviata in ripristino servizi Directory modalità ().
    > -   Per aumentare la disponibilità del servizio di dominio Active Directory, questa guida consiglia e include istruzioni con due host Hyper-V diversi, in modo da evitare un potenziale singolo punto di errore. Tuttavia, non è necessario due host Hyper-V per eseguire la clonazione di controller di dominio virtuale.
    > -   È necessario essere un membro del gruppo Administrators locale in ogni server Hyper-V (**HyperV1** e **HyperV2**).
    > -   Per importare ed esportare un file VHD utilizzando Hyper-V correttamente, i commutatori di rete virtuale in entrambi gli host Hyper-V devono avere lo stesso nome. Ad esempio, se si dispone di una commutatore di rete virtuale **HyperV1** deve essere attiva una rete virtuale denominato VNet **HyperV2** denominato VNet.
    > -   Se i due host Hyper-V (**HyperV1** e **HyperV2**) dispongono di processori diversi, arrestare la macchina virtuale (**VirtualDC1**) che si intende esportare, fare clic sulla macchina virtuale, fare clic su **impostazioni**, fare clic su **processore**e in **compatibilità processore** selezionare **eseguire la migrazione di un computer fisico con una versione diversa del processore** e fare clic su **OK**.

-   Un distribuito Windows Server 2012 controller di dominio (virtualizzato o fisico) che ospita il ruolo emulatore PDC (**DC1**). Per verificare se il ruolo emulatore PDC è ospitato in un controller di dominio Windows Server 2012, eseguire il comando di Windows PowerShell seguente:

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    Il valore di OperatingSystemVersion restituito deve essere una versione 6.2. Se necessario, è possibile trasferire il ruolo emulatore PDC a un controller di dominio che esegue Windows Server 2012. Per ulteriori informazioni, vedere [Using Ntdsutil.exe per assegnare o trasferire ruoli FSMO a un controller di dominio](https://support.microsoft.com/kb/255504).

-   Un controller di dominio virtualizzato guest Windows Server 2012 distribuiti (**VirtualDC1**) che è nello stesso dominio del controller di dominio Windows Server 2012 che ospita il ruolo emulatore PDC (**DC1**). Questo sarà il controller di dominio di origine utilizzato per la clonazione. Il controller di dominio virtuale guest verrà ospitato in un server Windows Server 2012 Hyper-V (**HyperV1**).

    > [!NOTE]
    > -   Per la clonazione riesca, il controller di dominio di origine che viene utilizzato per creare il clone non può essere da un controller di dominio viene abbassato poiché è stato creato il supporto VHD di origine.
    > -   Arrestare il controller di dominio di origine prima di copiare la macchina virtuale o il disco rigido virtuale.
    > -   Non deve essere clonare un disco rigido virtuale o ripristinare uno snapshot meno il valore della durata di rimozione definitiva (o il valore della durata di oggetti eliminati se è abilitato Cestino per Active Directory). Se si copia un disco rigido virtuale di un controller di dominio esistente, assicurarsi che il file VHD non sia meno recente che la durata di rimozione valore (per impostazione predefinita, 60 giorni). Non è necessario copiare un disco rigido virtuale di un controller di dominio in esecuzione per creare il supporto clone.

    Rimuovere eventuali unità floppy virtuale (VFD) di origine potrebbe essere controller di dominio. Ciò può provocare un problema di condivisione quando si tenta di importare la nuova macchina virtuale.

    Solo controller di dominio di Windows Server 2012 ospitato in un hypervisor VM-GenerationID è utilizzabile come origine per la clonazione. Windows Server 2012 controller di dominio utilizzato per la clonazione deve essere in uno stato integro. Per determinare lo stato del controller di dominio di origine eseguire [dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx). Per ottenere una migliore comprensione dell'output restituito da dcdiag, vedere [eseguite effettivamente... si?](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    Se il controller di dominio di origine è un server DNS, controller di dominio clonato sarà anche un server DNS. È consigliabile scegliere un server DNS che zone host solo integrate in Active Directory.

    Le impostazioni client DNS non vengono clonate, ma sono specificate nel file DCCloneConfig.xml. Se non vengono specificate, il controller di dominio clonato punterà a se stesso come server DNS preferito per impostazione predefinita. Il controller di dominio clonato non disporrà di una delega DNS. L'amministratore della zona DNS padre deve aggiornare la delega DNS per il controller di dominio clonato in base alle esigenze.

    > [!WARNING]
    > Le misure di sicurezza di virtualizzazione non si estendono ad Active Directory Lightweight Directory Services (AD LDS). Di conseguenza non tentare di clonare un controller di dominio Active Directory che ospita un'istanza di AD LDS aggiungendo questa istanza di AD LDS a CustomDCCloneAllowList.xml. Poiché AD LDS non è compatibile con ID di VM-Generation, la clonazione di un controller di dominio con AD LDS può causare una divergenza indotta dal rollback degli USN sul tale AD LDS set di configurazione.

    I seguenti ruoli del server non sono supportati per la clonazione:

    -   Dynamic Host Configuration Protocol (DHCP)

    -   Servizi certificati Active Directory (AD CS)

    -   Active Directory Lightweight Directory Services (AD LDS)

### <a name="bkmk4_grant_source"></a>Passaggio 1: Il controller di dominio virtualizzato l'autorizzazione per la clonazione
In questa procedura, il controller di dominio di origine l'autorizzazione per la clonazione utilizzando **centro di amministrazione di Active Directory** aggiungere controller di dominio di origine per il **controller di dominio clonabili** gruppo.

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>Per concedere l'autorizzazione per la clonazione di controller di dominio virtualizzati

1.  In qualsiasi controller di dominio nello stesso dominio del controller di dominio preparato per la clonazione (**VirtualDC1**), aprire **centro di amministrazione di Active Directory** (centro), individuare l'oggetto controller di dominio virtualizzati (controller di dominio sono in genere si trova sotto il **i controller di dominio** contenitore nel centro), fare clic su di esso, scegliere **Aggiungi al gruppo** e in **immettere il nome dell'oggetto da selezionare** tipo **controller di dominio clonabili** e quindi fare clic su **OK**.

    L'aggiornamento di appartenenza al gruppo eseguita in questo passaggio è necessario replicare emulatore PDC prima di poter eseguire la clonazione. Se il **controller di dominio clonabili** gruppo non viene trovato, il ruolo emulatore PDC potrebbe non essere ospitato in un controller di dominio che esegue Windows Server 2012.

    > [!NOTE]
    > Per aprire Centro in un controller di dominio Windows Server 2012, aprire Windows PowerShell e digitare **dsac.exe**.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il cmdlet di Windows PowerShell seguente esegue la stessa funzione della procedura precedente:


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>Passaggio 2: Esegui Get-ADDCCloningExcludedApplicationList cmdlet
In questa procedura, eseguire il `Get-ADDCCloningExcludedApplicationList`cmdlet nel controller di dominio virtualizzato di origine per identificare tutti i programmi o servizi non valutati per la clonazione. È necessario eseguire il cmdlet Get-ADDCCloningExcludedApplicationList prima il cmdlet New-ADDCCloneConfigFile perché se il cmdlet New-ADDCCloneConfigFile rileva un'applicazione esclusa, non creerà un file DCCloneConfig.xml.

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>Per identificare le applicazioni o servizi in esecuzione su un controller di dominio di origine che non sono stati valutati per la clonazione

1.  Nel controller di dominio di origine (**VirtualDC1**), fare clic su **Server Manager**, fare clic su **strumenti**, fare clic su **modulo Active Directory per Windows PowerShell** e quindi digitare il comando seguente:


    Get-ADDCCloningExcludedApplicationList


2.  Esaminare l'elenco dei servizi restituiti e i programmi installati con il fornitore del software per determinare se che possono essere clonati in modo sicuro. Se le applicazioni o servizi nell'elenco non possono essere clonati in modo sicuro, è necessario rimuoverli dal controller di dominio di origine o la clonazione non riuscirà.

3.  Per il set di servizi e programmi installati che sono stato sviluppati per essere clonati in modo sicuro, eseguire il comando con il **"GenerateXML** switch per eseguire il provisioning di questi servizi e programmi nel **CustomDCCloneAllowList.xml** file.


    Get-ADDCCloningExcludedApplicationList - GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>Passaggio 3: Eseguire New-ADDCCloneConfigFile
Eseguire New-ADDCCloneConfigFile nel controller di dominio di origine e facoltativamente le impostazioni di configurazione per il controller di dominio clone, ad esempio il nome, indirizzo IP e il resolver DNS.

Ad esempio, per creare un controller di dominio clone denominato VirtualDC2 con un indirizzo IPv4 statico, digitare:


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> Il controller di dominio clone possono trovarsi nello stesso sito come controller di dominio di origine a meno che non viene specificato un altro sito nel file DCCloneConfig.xml. Si consiglia di specificare un sito appropriato nel file DCCloneConfig.xml per il controller di dominio clone in base al relativo indirizzo IP.

Il nome del computer è facoltativo. Se non si specifica uno, verrà generato un nome univoco in base all'algoritmo seguente:

-   Il prefisso è i primi 8 caratteri del nome di computer controller di dominio di origine. Ad esempio, un nome di computer di origine sourcecomputer viene troncato a una stringa prefisso SourceCo.

-   Un suffisso di denominazione univoco del formato "" CL*nnnn*"viene aggiunto alla stringa di prefisso in cui *nnnn*è il successivo valore disponibile da 0001-9999 che determina il PDC non è attualmente in uso. Ad esempio, se 0047 è il successivo numero disponibile nell'intervallo consentito, utilizzando l'esempio precedente del prefisso del nome computer SourceCo, il nome derivato da utilizzare per il computer clone verrà impostato come SourceCo-CL0047.

> [!NOTE]
> Server di catalogo globale (GC) è necessario per il cmdlet New-ADDCCloneConfigFile funzioni correttamente. L'appartenenza del controller di dominio di origine il **controller di dominio clonabili** gruppo deve riflettersi sul catalogo globale. Il catalogo globale non deve essere lo stesso controller di dominio dell'emulatore PDC, ma deve preferibilmente trovarsi nello stesso sito. Se un catalogo globale non è disponibile, il comando ha esito negativo con l'errore "il server non è operativo." Per ulteriori informazioni, vedere [virtualizzato risoluzione dei problemi Controller di dominio](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).

Per creare un controller di dominio clone denominato Clone1 con impostazioni IPv4 statiche e specificare i server WINS preferito e alternativo, digitare:


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> Se si specificano server WINS, è necessario specificare sia **"PreferredWINSServer** e **" AlternateWINSServer**. Se si specifica solo di questi argomenti, la clonazione non riesce con errore 0x80041005 di codice nel dcpromo.log.

Per creare un controller di dominio clone denominato Clone2 con impostazioni IPv4 dinamiche, digitare:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> In questo caso, dovrebbe esserci un server DHCP nell'ambiente che il clone può raggiungere e ottenere l'indirizzo IP e altre impostazioni di rete pertinenti.

Per creare un controller di dominio clone denominato Clone2 con impostazioni IPv4 dinamiche e specificare i server WINS preferito e alternativo, digitare:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


Per creare un controller di dominio clone con impostazioni IPv6 dinamiche, digitare:


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


Per creare un controller di dominio clone con impostazioni IPv6 statiche, digitare:


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> Quando si specificano impostazioni IPv6, l'unica differenza tra le impostazioni statiche e dinamiche è l'inclusione di **-statico** passare. L'inclusione del **-statico** commutatore rende obbligatorio specificare almeno un **IPv6DNSResolver**. L'indirizzo IPv6 statico deve essere configurato tramite la configurazione automatica indirizzo senza stato (SLAAC) con prefissi assegnati dal router. Con IPv6 dinamico, il resolver DNS è facoltativo, ma è previsto che il clone possa raggiungere un server DHCP abilitato per IPv6 nella subnet per ottenere l'indirizzo IPv6 e informazioni sulla configurazione DNS.

#### <a name="BKMK_OfflineMode"></a>Esecuzione New-ADDCCloneConfigFile in modalità offline
Se si dispongano di più copie di supporto controller di dominio di origine preparate per la clonazione (vale a dire il controller di dominio di origine è autorizzato per la clonazione, il cmdlet Get-ADDCCloningExcludedApplicationList è stato eseguito e così via) e si desidera specificare impostazioni diverse per ogni copia del supporto, è possibile eseguire New-ADDCCloneConfigFile in modalità offline. Questo può essere più efficiente singola preparazione di ogni macchina virtuale, ad esempio, tramite l'importazione di ogni copia.

In questo caso, gli amministratori di dominio possono montare il disco offline e utilizzare strumenti di amministrazione remota Server (RSAT) per eseguire il cmdlet New-ADDCCloneConfigFile con l'argomento - offline allo scopo di aggiungere i file XML, che consente l'automazione di factory simile con nuove opzioni di Windows PowerShell incluse in Windows Server 2012. Per ulteriori informazioni su come montare il disco offline per eseguire il cmdlet New-ADDCCloneConfigFile in modalità offline, vedere [aggiunta di XML al disco di sistema Offline](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline).

È prima necessario eseguire il cmdlet localmente nel supporto di origine per garantire che superamento dei controlli dei prerequisiti. I controlli dei prerequisiti non vengono eseguiti in modalità offline, perché il cmdlet può essere eseguito da un computer che potrebbe non essere dello stesso dominio o da un computer aggiunto al dominio. Dopo aver eseguito il cmdlet localmente, creerà un file DCCloneConfig.xml. È possibile eliminare DCCloneConfig.xml creato localmente se si prevede di utilizzare la modalità offline successivamente.

Per creare un controller di dominio clone denominato CloneDC1 in modalità offline, in un sito denominato REDMOND"con indirizzo IPv4 statico, tipo:


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


Per creare un controller di dominio clone denominato Clone2 in modalità offline con impostazioni IPv4 statiche e statiche IPv6, tipo:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


Per creare un controller di dominio clone in modalità offline con impostazioni IPv4 statiche e dinamiche IPv6 e specificare più server DNS per le impostazioni del resolver DNS, digitare:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


Per creare un controller di dominio clone denominato Clone1 in modalità offline con impostazioni IPv4 dinamiche e statiche IPv6, tipo:


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


Per creare un controller di dominio clone in modalità offline con impostazioni IPv4 dinamiche e dinamico IPv6, digitare:


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>Passaggio 4: Esportare e quindi importare la macchina virtuale del controller di dominio di origine
In questa procedura, esportare la macchina virtuale del controller di dominio virtualizzato di origine e quindi importare la macchina virtuale. Questa azione crea un controller di dominio virtualizzato clone nel dominio.

È necessario essere un membro del gruppo Administrators locale in ogni host Hyper-V. Se si utilizzano credenziali diverse per ogni server, eseguire i cmdlet di Windows PowerShell per esportare e importare la macchina virtuale in diverse sessioni di Windows PowerShell.

Se nel controller di dominio di origine sono presenti snapshot, devono essere eliminati prima che il controller di dominio di origine viene esportato, in quanto la macchina virtuale non verrà importata Se uno snapshot include impostazioni del processore sono compatibili con l'host hyper-v di destinazione. Se le impostazioni del processore sono compatibili tra gli host hyper-v di origine e di destinazione, è possibile esportare e copiare l'origine senza eliminare prima gli snapshot. Dopo l'importazione, tuttavia, gli snapshot eliminarli dalla macchina virtuale clone prima che venga avviato.

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>Per copiare un controller di dominio virtuale esportando e poi importando il controller di dominio virtualizzati

1.  In **HyperV1**, arresto il controller di dominio di origine (**VirtualDC1**).

    ![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

    Stop-VM-nome VirtualDC1 - ComputerName HyperV1


2.  In **HyperV1**, eliminare gli snapshot e quindi esportare il controller di dominio di origine (VirtualDC1) nella directory c:\clonedcs..

> [!NOTE]
> Poiché ogni volta che viene eseguito uno snapshot, viene creato un nuovo file AVHD che funge da disco differenze, è consigliabile eliminare tutti gli snapshot associati. Consente di creare un effetto a catena. Se si stati creati snapshot e inserire il file DCCLoneConfig.xml nel disco rigido virtuale, si rischia di creare un clone da una versione precedente di DIT o di inserire il file di configurazione nel file VHD non corretto. Eliminare lo snapshot unisce tutti questi file avhd nel file VHD di base.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  Copiare la cartella **virtualdc1** nella directory c:\Import di **HyperV2**.

4.  In **HyperV2**, l'uso **gestione di Hyper-V**, importare la macchina virtuale (utilizzando il **Importa macchina virtuale** procedura guidata in **gestione di Hyper-V**) dalla cartella **c:\Import\virtualdc1** ed Elimina tutti associata **snapshot**.

Utilizzare il **copia macchina virtuale (Crea nuovo ID univoco)** opzione quando si importa la macchina virtuale.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


Per creare un clone più controller di dominio dallo stesso controller di dominio di origine:

  -   Interfaccia utente: il **Importa macchina virtuale** procedura guidata, specificare nuovi percorsi per **cartella di configurazione macchina virtuale**, **archivio Snapshot**, **file di paging intelligente**e un altro **percorso** per i dischi rigidi virtuali per la macchina virtuale.

  -   Windows PowerShell: specificare nuovi percorsi per la macchina virtuale utilizzando i seguenti parametri per il `Import-VM`cmdlet:

        $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual macchine" Import-VM-percorso $path.fullname-copia - GenerateNewId - ComputerName HyperV2 - VhdDestinationPath "path" - SnapshotFilePath "percorso" - SmartPagingFilePath "path" - VirtualMachinePath "path"


> [!NOTE]
> La dimensione batch consigliata per la creazione contemporaneamente i controller di dominio più clone è 10. Il numero massimo è limitato dal numero massimo di connessioni di replica in uscita, che per impostazione predefinita è 16 per DFSR Distributed File System Replication () e 10 per servizio Replica File (FRS). Non è necessario distribuire più il numero consigliato di controller di dominio clone contemporaneamente a meno che non è stato testato accuratamente tale numero per l'ambiente.

5.  In **HyperV1**, riavviare il controller di dominio di origine (**(VirtualDC1**) per riportarlo online.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  In **HyperV2**, avviare la macchina virtuale (**VirtualDC2**) per portarla online come controller di dominio clone nel dominio.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> L'emulatore PDC deve essere in esecuzione per completare la clonazione. Se è stato arrestato, assicurarsi che è stato avviato ed eseguito la sincronizzazione iniziale in modo che riconosca che contiene il ruolo emulatore PDC. Per ulteriori informazioni, vedere Microsoft [articolo della Knowledge Base 305476](https://support.microsoft.com/kb/305476).

Al termine della clonazione, verificare il nome del computer clone per garantire l'operazione di clonazione completata. Verificare che la macchina virtuale non è stato avviato nel ripristino servizi Directory modalità (). Se si tenta di accedere e ricevere un errore che indica che nessun server di accesso è disponibile, provare ad accedere in modalità ripristino servizi directory. Se il controller di dominio non è stato clonato correttamente e viene avviato in modalità ripristino servizi directory, controllare i registri nel Visualizzatore eventi e i log dcpromo nella cartella %systemroot%/debug.

Il controller di dominio clonato sarà un membro del **controller di dominio clonabili** gruppo perché copia l'appartenenza dal controller di dominio di origine. Come procedura consigliata, è consigliabile lasciare il **controller di dominio clonabili** gruppo vuoto finché non si è pronti per eseguire operazioni di clonazione e dopo le operazioni di clonazione, è necessario rimuovere membri.

Se il controller di dominio di origine archivia un supporto di backup, il controller di dominio clonato archivierà anche il supporto di backup. È possibile eseguire `wbadmin get versions`per mostrare il supporto di backup nel controller di dominio clonato. Un membro del gruppo Domain Admins deve eliminare il supporto di backup nel controller di dominio clonato per impedirne ripristino accidentale. Per ulteriori informazioni su come eliminare un backup dello stato del sistema utilizzando wbadmin.exe, vedere [Wbadmin delete systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx).

## <a name="troubleshooting"></a>Risoluzione dei problemi
Se il controller di dominio clone (**VirtualDC2**) viene avviato in modalità servizi Directory ripristinare (DSRM), non viene restituito in una modalità normale al successivo riavvio. Per accedere a un controller di dominio avviato in modalità ripristino servizi directory, utilizzare **. \Administrator** e specificare la password modalità ripristino servizi directory.

Correggere la causa errore di clonazione e verificare che il dcpromo.log non indica che non è possibile riprovare la clonazione. Se non è possibile riprovare la clonazione, sostituire in modo sicuro il supporto. Se la clonazione può essere ritentata, è necessario rimuovere il flag di avvio in modalità ripristino servizi directory per riprovare la clonazione.

1.  Aprire Windows Server 2012 con un comando con privilegi elevato (destra fare clic su Windows Server 2012 e scegliere Esegui come amministratore) e quindi digitare **msconfig**.

2.  Nel **avvio** nella scheda **opzioni di avvio**, deselezionare **avvio in modalità provvisoria** (è già selezionata con l'opzione **Ripristina Active Directory abilitata**).

3.  Fare clic su **OK** e riavviare quando richiesto.

Per ulteriori informazioni sulla risoluzione dei problemi su controller di dominio virtualizzati, vedere [virtualizzato risoluzione dei problemi Controller di dominio](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).


