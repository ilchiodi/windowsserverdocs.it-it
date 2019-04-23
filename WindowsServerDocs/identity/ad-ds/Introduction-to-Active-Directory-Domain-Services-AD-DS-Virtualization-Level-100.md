---
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
title: Introduzione alla virtualizzazione di Active Directory Domain Services (livello 100)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b818ba5a58db38bdb3c0f630a8d9d2daa1494403
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878092"
---
# <a name="introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100"></a>Introduzione alla virtualizzazione di Active Directory Domain Services (livello 100)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La virtualizzazione di ambienti Servizi di dominio Active Directory viene sviluppata da diversi anni. A partire da Windows Server 2012, Active Directory Domain Services offre maggiore supporto per la virtualizzazione dei controller di dominio grazie all'introduzione di funzionalità di virtualizzazione.

## <a name="safe-virtualization-of-domain-controllers"></a>Virtualizzazione sicura dei controller di dominio

Gli ambienti virtualizzati pongono sfide straordinarie per i carichi di lavoro distribuiti che dipendono da uno schema di replica basato su clock logico. La replica di Servizi di dominio Active Directory, ad esempio, utilizza un valore con incremento monotono, denominato numero di sequenza di aggiornamento o USN, assegnato alle transazioni in ogni controller di dominio. Istanza del ogni controller di dominio database viene inoltre assegnata un'identità, denominata ID chiamata. L'ID chiamata di un controller di dominio e il relativo USN fungono insieme da identificatore univoco associato a ogni transazione di scrittura eseguita in ogni controller di dominio e devono essere univoci all'interno della foresta.

La replica di Servizi di dominio Active Directory utilizza l'ID chiamata e gli USN in ogni controller di dominio per determinare le modifiche che è necessario replicare in altri controller di dominio. Se un controller di dominio verrà riportato nel tempo all'esterno di presenza del controller di dominio e viene riutilizzato un USN per una transazione interamente diversa, la replica non convergerà perché altri controller di dominio verrà ritiene di che avere già ricevuto l'aggiornamento associato all'USN riutilizzato nel contesto di tale ID.

Nell'illustrazione seguente, ad esempio, viene mostrata la sequenza di eventi che si verifica in Windows Server 2008 R2 e nei sistemi operativi precedenti quando il rollback degli USN viene rilevato in VDC2, ovvero il controller di dominio di destinazione eseguito in una macchina virtuale. In questa illustrazione, il rilevamento del rollback degli USN si verifica su VDC2 quando un partner di replica rileva che VDC2 ha inviato un valore USN di aggiornamento che è stato individuato in precedenza dal partner di replica, a indicare che il database di VDC2 ha rollback nel tempo in modo non corretto.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Una macchina virtuale (VM) semplifica per gli amministratori degli hypervisor ripristinare un dominio USN del controller (il clock logico), ad esempio, l'applicazione di uno snapshot di fuori di presenza del controller di dominio. Per ulteriori informazioni sugli USN e USN rollback, inclusa un'altra illustrazione per illustrare le istanze non rilevate del rollback degli USN, vedere [USN e Rollback degli USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

A partire da Windows Server 2012, il controller di dominio virtuali AD DS ospitato in piattaforme hypervisor che espongono un identificatore denominato ID di generazione VM può rilevare e utilizzare misure di sicurezza necessarie per proteggere l'ambiente di dominio Active Directory se la macchina virtuale è rollback in tempo per l'applicazione di uno snapshot della macchina Virtuale. Questa progettazione utilizza un meccanismo indipendente dal fornitore dell'hypervisor per esporre l'identificatore nello spazio degli indirizzi della macchina virtuale guest, in modo da garantire un'esperienza di virtualizzazione sicura in qualsiasi hypervisor che supporta tale ID di generazione macchina virtuale. Questo identificatore può essere campionato da servizi e applicazioni eseguiti all'interno della macchina virtuale per rilevare se è stato eseguito il rollback di una macchina virtuale in tempo.

### <a name="BKMK_HowSafeguardsWork"></a>Come funzionano queste misure di sicurezza di virtualizzazione?
Durante l'installazione di controller di dominio Active Directory archivia inizialmente l'identificatore di generazione macchina Virtuale come parte dell'attributo msDS-GenerationID sull'oggetto computer del controller di dominio nel proprio database (noto anche come la directory information tree o DIT). L'ID di generazione macchina virtuale viene registrato in modo indipendente da un driver Windows all'interno della macchina virtuale.

Quando un amministratore ripristina la macchina virtuale da uno snapshot precedente, il valore corrente dell'ID di generazione macchina virtuale ottenuto dal driver della macchina virtuale viene confrontato con un valore nel nell'albero delle informazioni di directory.

Se i due valori sono diversi, l'ID chiamata viene reimpostato e il pool di RID viene ignorato impedendo in tal modo il riutilizzo dell'USN. Se i valori sono identici, viene eseguito il commit della transazione come normale.

Servizi di dominio Active Directory confronta inoltre il valore corrente dell'ID di generazione macchina virtuale della macchina virtuale con il valore nell'albero delle informazioni di directory ogni volta che viene riavviato il controller di dominio e, se diverso, reimposta l'ID chiamata, ignora il pool di RID e aggiorna l'albero delle informazioni di directory con il nuovo valore. Sincronizza inoltre in modo non autorevole la cartella SYSVOL per completare il ripristino sicuro. In questo modo le misure di sicurezza possono estendersi all'applicazione di snapshot nelle macchine virtuali arrestate. Queste misure di sicurezza introdotti in Windows Server 2012 consentono agli amministratori di dominio Active Directory beneficiare dei vantaggi univoci di distribuzione e gestione di controller di dominio in un ambiente virtualizzato.

Nella figura seguente viene illustrato come vengono applicate misure di sicurezza della virtualizzazione quando viene rilevato il rollback degli USN stesso in un controller di dominio virtualizzati che esegue Windows Server 2012 in un hypervisor che supporta VM-GenerationID.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

In questo caso, quando l'hypervisor rileva una modifica al valore dell'ID di generazione macchina virtuale, vengono attivate misure di sicurezza di virtualizzazione, inclusi la reimpostazione dell'ID chiamata per il controller di dominio virtualizzato (da A a B nell'esempio precedente) e l'aggiornamento del valore dell'ID di generazione macchina virtuale salvato nella macchina virtuale perché corrisponda al nuovo valore (G2) archiviato nell'hypervisor. Le misure di sicurezza fanno sì che la replica converga per entrambi i controller di dominio.

Con Windows Server 2012, Active Directory utilizza misure di sicurezza nei controller di dominio virtuali ospitati in hypervisor compatibile con ID di generazione VM e garantisce che l'applicazione accidentale di snapshot o altri sistemi analoghi di abilitati per hypervisor che potrebbero eseguire il rollback dello stato di una macchina virtuale non interferire con l'ambiente di dominio Active Directory (per evitare problemi di replica, ad esempio una bolla USN o oggetti residui). Il ripristino di un controller di dominio tramite l'applicazione di uno snapshot macchina virtuale non è tuttavia consigliato come meccanismo alternativo per il backup di un controller di dominio. È consigliabile continuare a utilizzare Windows Server Backup o altre soluzioni di backup basate sul writer del servizio Copia Shadow.

> [!CAUTION]
> Se un controller di dominio in un ambiente di produzione viene accidentalmente ripristinato a uno snapshot, è consigliabile che rivolgersi ai fornitori per le applicazioni e servizi ospitati su tale macchina virtuale, per istruzioni su come verificare lo stato di tali programmi dopo snapshot ripristino.

Per ulteriori informazioni, vedere [architettura di ripristino sicuro di controller di dominio virtualizzati](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="virtualized_dc_cloning"></a>Clonazione del controller di dominio virtualizzati
A partire da Windows Server 2012, gli amministratori possono facilmente e in modo sicuro distribuire controller di dominio di replica copiando un controller di dominio virtuale esistente. In un ambiente virtuale gli amministratori non devono più distribuire ripetutamente un'immagine del server preparata utilizzando sysprep.exe, promuovere di livello il server a un controller di dominio e quindi completare requisiti di configurazione aggiuntivi per distribuire ogni controller di dominio di replica.

> [!NOTE]
> Gli amministratori devono seguire processi esistenti per distribuire il primo controller di dominio in un dominio, ad esempio utilizzare sysprep.exe per preparare un disco rigido virtuale del server, promuovere di livello il server a un controller di dominio e quindi completare eventuali requisiti di configurazione aggiuntivi. In uno scenario di ripristino di emergenza utilizzare il backup del server più recente per ripristinare il primo controller di dominio in un dominio.

### <a name="scenarios-that-benefit-from-virtual-domain-controller-cloning"></a>Scenari in cui è utile la clonazione di controller di dominio virtuali

-   Distribuzione rapida di controller di dominio aggiuntivi in un nuovo dominio

-   Veloce recupero della continuità aziendale durante il ripristino di emergenza ripristinando la capacità di Servizi di dominio Active Directory tramite una rapida distribuzione dei controller di dominio con la clonazione

-   Ottimizzazione delle distribuzioni di cloud privati tramite il provisioning elastico dei controller di dominio per soddisfare i requisiti di scala maggiore

-   Rapido provisioning di ambienti di test che consente la distribuzione e i test di nuove caratteristiche e funzionalità prima dell'implementazione nell'ambiente di produzione

-   Veloce risposta alle esigenze di maggiore capacità nelle succursali tramite la clonazione di controller di dominio esistenti nelle succursali

Quando si distribuisce rapidamente un numero elevato di controller di dominio, continuare a eseguire le procedure esistenti per la convalida dello stato di ogni controller di dominio al termine dell'installazione. Distribuire i controller di dominio in batch di dimensioni ragionevoli in modo da poter convalidarne lo stato al termine di ogni batch di installazioni. La dimensione batch consigliata è 10. Per ulteriori informazioni, vedere [passaggi per la distribuzione di un controller di dominio virtualizzato clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc).

### <a name="clear-separation-of-responsibilities"></a>Netta separazione delle responsabilità
L'autorizzazione a clonare controller di dominio virtualizzati è controllata dall'amministratore di Servizi di dominio Active Directory. Perché gli amministratori degli hypervisor possano distribuire controller di dominio aggiuntivi copiando controller di dominio virtuali, l'amministratore di Servizi di dominio Active Directory deve selezionare e autorizzare un controller di dominio e quindi eseguire i passaggi preparatori per abilitarlo come origine per la clonazione.

Assegnando in genere il provisioning delle macchine virtuali all'amministratore dell'hypervisor, gli amministratori degli hypervisor possono eseguire il provisioning delle macchine virtuali del controller di dominio di replica copiando controller di dominio virtualizzati che siano stati autorizzati e preparati per la clonazione dall'amministratore di Servizi di dominio Active Directory.

> [!WARNING]
> Chiunque sia autorizzato ad amministrare l'hypervisor che ospita un controller di dominio virtuale deve essere altamente attendibile e controllato nell'ambiente.

### <a name="how-does-virtual-domain-controller-cloning-work"></a>Funzionamento della clonazione del controller di dominio virtuale
Il processo di clonazione implica la copia del disco rigido Virtuale del controller di dominio virtuale un esistente (oppure, per le configurazioni più complesse, la macchina Virtuale del controller di dominio), l'autorizzazione per la clonazione di dominio Active Directory e la creazione di un file di configurazione clone. In questo modo viene ridotto il numero di passaggi e il tempo necessari per la distribuzione di un controller di dominio virtuale di replica tramite l'eliminazione di attività di distribuzione altrimenti ripetitive.

Il controller di dominio clone utilizza i criteri seguenti per rilevare che si tratta di una copia di un altro controller di dominio:

1.  Il valore dell'ID di generazione macchina virtuale fornito dalla macchina virtuale è diverso dal valore dell'ID archiviato nell'albero delle informazioni di directory (DIT).

    > [!NOTE]
    > La piattaforma hypervisor deve supportare l'ID di generazione VM (Windows Server 2012 Hyper-V supporta l'ID di generazione VM).

2.  Presenza di un file denominato DCCloneConfig.xml in uno dei percorsi seguenti:

    -   Directory in cui si trova l'albero delle informazioni di directory

    -   %windir%\ntds\

    -   Radice di un'unità rimovibile

Una volta soddisfatti i criteri, il controller di dominio clone esegue il processo di clonazione per eseguire il provisioning di se stesso come controller di dominio di replica.

Il controller di dominio clone utilizza il contesto di sicurezza del controller di dominio di origine (il cui rappresenta una copia di controller di dominio) per contattare il titolare di ruolo di master di Windows Server 2012 Domain Controller primario (PDC) emulatore operazioni (noto anche come FSMO, FSMO, Flexible). L'emulatore PDC deve essere in esecuzione Windows Server 2012, ma non deve essere eseguito su un hypervisor.

> [!NOTE]
> Se si dispone di un'estensione dello schema con attributi che fanno riferimento al controller di dominio di origine e l'attributo è uno degli oggetti copiati (oggetto computer, oggetto Impostazioni NTDS) per creare il clone, tale attributo non verrà copiato o aggiornato per fare riferimento al controller di dominio clone.

Dopo aver verificato che il controller di dominio che invia la richiesta è autorizzato per la clonazione, l'emulatore PDC crea una nuova identità macchina includendo un nuovo account, un SID, un nome e una password che identifichino la macchina come controller di dominio di replica e inviano di nuovo queste informazioni al clone. Il controller di dominio clone prepara quindi i file del database di Servizi di dominio Active Directory perché fungano da replica ed esegue la pulizia dello stato della macchina.

Per ulteriori informazioni, vedere l'articolo relativo all'[architettura di clonazione dei controller di dominio virtualizzati](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch).

### <a name="cloning-components"></a>Componenti di clonazione
Tra i componenti di clonazione sono inclusi nuovi cmdlet nel modulo Active Directory per Windows PowerShell e i file XML associati:

-   **New-ADDCCloneConfigFile** "questo cmdlet crea e inserisce dccloneconfig. XML nella posizione corretta per assicurarsi che sia disponibile per attivare la clonazione. Esegue inoltre controlli dei prerequisiti per garantire la corretta clonazione. È incluso nel modulo Active Directory per Windows PowerShell. È possibile eseguire questo cmdlet localmente in un controller di dominio virtualizzato preparato per la clonazione oppure in remoto utilizzando l'opzione -offline. È possibile specificare impostazioni per il controller di dominio clone, ad esempio il nome, il sito e l'indirizzo IP.

    I controlli dei prerequisiti eseguiti sono i seguenti:

    > [!NOTE]
    > I controlli dei prerequisiti non vengono eseguite quando la "opzione offline è utilizzato. Per ulteriori informazioni, vedere [Esecuzione di New-ADDCCloneConfigFile in modalità offline](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#BKMK_OfflineMode).

    -   Il controller di dominio preparato è autorizzato per la clonazione, ovvero è membro del gruppo **Controller di dominio clonabili**.

    -   L'emulatore PDC esegue Windows Server 2012.

    -   Tutti i programmi o servizi elencati per l'esecuzione di **Get-ADDCCloningExcludedApplicationList** sono inclusi in CustomDCCloneAllowList.xml (descritto in modo più dettagliato alla fine dell'elenco dei componenti di clonazione).

-   **Dccloneconfig** "per clonare correttamente un controller di dominio virtualizzati, questo file deve essere presente nella directory in cui risiede il DIT, *%windir%\NTDS*, o la radice di un'unità rimovibile. Oltre a essere utilizzato come uno dei trigger per rilevare e avviare la clonazione, consente inoltre di specificare impostazioni di configurazione per il controller di dominio clone.

    Lo schema e un file di esempio per il file dccloneconfig. XML vengono archiviati in tutti i computer Windows Server 2012 in:

    -   %windir%\system32\DCCloneConfigSchema.xsd

    -   %windir%\system32\SampleDCCloneConfig.xml

    È consigliabile utilizzare il cmdlet New-ADDCCloneConfigFile per creare il file DCCloneConfig.xml. Benché sia anche possibile utilizzare il file dello schema con un editor XML per creare il file, la modifica manuale del file aumenta la probabilità di errori. Se si modifica il file, è necessario usare editor XML, ad esempio Visual Studio, [XML Notepad](https://www.microsoft.com/download/details.aspx?displaylang=en&id=7973)o applicazioni di terze parti, ma non Blocco note.

-   **Get-ADDCCloningExcludedApplicationList** "questo cmdlet viene eseguito sul controller di dominio di origine prima di iniziare il processo di clonazione per determinare quali servizi o programmi installati non inclusi nell'elenco supportati predefinito, defaultdccloneallowlist. XML, o un'inclusione definito dall'utente elenco denominato file customdccloneallowlist. XML e pertanto non sono stati valutati per l'impatto della clonazione.

    Questo cmdlet cerca nel controller di dominio di origine i servizi in Gestione controllo servizi e i programmi installati elencati in **HKLM\Software\Microsoft\Windows\CurrentVersion\Uninstall** che non sono specificati nell'elenco predefinito (DefaultDCCloneAllowList.xml) o, se disponibile, nell'elenco di inclusione definito dall'utente (CustomDCCloneAllowList.xml file). L'elenco di applicazioni e servizi restituito tramite l'esecuzione del cmdlet corrisponde alla differenza tra quanto già specificato nel file DefaultDCCloneAllowList.xml o CustomDCCloneAllowList.xml e l'elenco creato in fase di esecuzione, in base a ciò che è installato nel controller di dominio di origine. L'output dei servizi e programmi restituito da Get-ADDCCloningExcludedApplicationList può essere aggiunto al file CustomDCCloneAllowList.xml se si determina che i servizi e i programmi possono essere clonati in modo sicuro. Per determinare se un servizio o un programma installato può essere clonato in modo sicuro, valutare le condizioni seguenti:

    -   Se il servizio o il programma installato è interessato dall'identità macchina, come nome, SID, password e così via.

    -   Se il servizio o il programma installato archivia localmente nel computer stati che possono interessarne la funzionalità nel clone.

    È necessario verificare con il fornitore software dell'applicazione per determinare se il servizio o il programma possa essere clonato in modo sicuro.

    > [!NOTE]
    > Prima di eseguire il provisioning di servizi o programmi aggiuntivi nel file CustomDCCloneAllowList.xml, verificare se si dispone della licenza necessaria per copiare il software contenuto nella macchina virtuale.

    Se le applicazioni non sono clonabili, rimuoverle dal controller di dominio di origine prima di creare il supporto clone. Se un'applicazione è inclusa nell'output del cmdlet, ma non nel file CustomDCCloneAllowList.xml, la clonazione non riuscirà. Perché la clonazione riesca, l'output del cmdlet non deve includere alcun servizio o programma. In altri termini, un'applicazione deve essere inclusa nel file CustomDCCloneAllowList.xml oppure essere rimossa dal controller di dominio di origine.

    Nella tabella seguente vengono descritte le opzioni per l'esecuzione di Get-ADDCCloningExcludedApplicationList.

    |||
    |-|-|
    |Argomento|Spiegazione|
    |*<no argument specified>*|Visualizza un elenco di servizi o programmi nella console che non sono stati considerati per la clonazione. Se è già presente un file CustomDCCloneAllowList.XML in una delle posizioni consentite, viene utilizzato tale file per visualizzare i servizi e i programmi rimanenti, che possono essere zero se gli elenchi corrispondono.|
    |-GenerateXml|Crea il file CustomDCCloneAllowList.XML, in cui vengono inseriti i servizi e i programmi elencati nella console.|
    |-Force|Sovrascrive un file CustomDCCloneAllowList.XML esistente.|
    |-Path|Percorso della cartella per creare il file CustomDCCloneAllowList.XML.|

-   **Defaultdccloneallowlist** "questo file è presente per impostazione predefinita in ogni Windows Server 2012 domain controller nel *%windir%\system32*. Il file elenca i servizi e i programmi installati che possono essere clonati in modo sicuro per impostazione predefinita. Perché la clonazione riesca, è necessario evitare di modificare il percorso o il contenuto di questo file.

-   **Customdccloneallowlist** "Se si dispone di servizi o i programmi installati che si trovano nel controller di dominio di origine non compresi tra quelli elencati nel file defaultdccloneallowlist. XML, tali programmi e servizi devono essere incluso in questo file. Per trovare i servizi o i programmi installati non elencati nel file DefaultDCCloneAllowList.xml, eseguire il cmdlet **Get-ADDCCloningExcludedApplicationList** . Si consiglia di utilizzare il **"GenerateXml** argomento per generare il file XML.

    Durante il processo di clonazione vengono controllate le posizioni seguenti nell'ordine indicato per il file e viene utilizzato il primo file XML trovato, indipendentemente dall'altro contenuto della cartella:

    1.  La chiave del Registro di sistema seguente:

        ```
        HKey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters
        AllowListFolder (REG_SZ)
        ```

    2.  Directory di lavoro DSA

    3.  %systemroot%\NTDS

    4.  Supporti rimovibili di lettura/scrittura in ordine di lettera di unità, nella radice dell'unità

### <a name="deployment-scenarios"></a>Scenari di distribuzione
Gli scenari di distribuzione seguenti sono supportati per la clonazione dei controller di dominio virtuali:

-   Distribuire un controller di dominio clone eseguendo una copia del file di disco rigido virtuale (vhd) del controller di dominio di origine.

-   Distribuire un controller di dominio clone tramite la copia della macchina virtuale di un controller di dominio di origine utilizzando la semantica di esportazione/importazione esposta dall'hypervisor.

> [!NOTE]
> I passaggi nella sezione [passaggi per la distribuzione di un controller di dominio virtualizzato clone](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#steps_deploy_vdc) viene descritta la copia di una macchina virtuale utilizzando la funzionalità di esportazione/importazione di Windows Server 2012 Hyper-V.

## <a name="steps_deploy_vdc"></a>Passaggi per la distribuzione di un controller di dominio virtualizzato clone

-   [Prerequisiti](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#prerequisites)

-   [Passaggio 1: Concedere l'autorizzazione per la clonazione di controller di dominio virtualizzato di origine](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk4_grant_source)

-   [Passaggio 2: Eseguire il cmdlet Get-ADDCCloningExcludedApplicationList](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet)

-   [Passaggio 3: Eseguire New-ADDCCloneConfigFile](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk5_create_insert_dccloneconfig)

-   [Passaggio 4: Esportare e importare la macchina virtuale del controller di dominio di origine](../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md#bkmk7_export_import_vm_sourcedc)

### <a name="prerequisites"></a>Prerequisiti

-   Per completare i passaggi illustrati nelle procedure seguenti è necessario essere membri del gruppo Domain Admins o disporre di autorizzazioni equivalenti.

-   Da un prompt dei comandi con privilegi elevati, è necessario eseguire i comandi di Windows PowerShell utilizzati in questa Guida. A tale scopo, fare clic il **Windows PowerShell** icona e quindi fare clic su **Esegui come amministratore**.

-   Un server Windows Server 2012 con il ruolo server Hyper-V installato (**HyperV1**).

-   Un secondo server di Windows Server 2012 con il ruolo server Hyper-V installato (**HyperV2**).

    > [!NOTE]
    > -   Se si utilizza un altro hypervisor, è consigliabile rivolgersi al fornitore dell'hypervisor per verificare che supporti l'ID di generazione macchina virtuale. Se l'hypervisor non supporta l'ID di generazione macchina virtuale ed è stato fornito un file DCCloneConfig.xml, la nuova macchina virtuale verrà avviata in modalità ripristino servizi directory.
    > -   Per aumentare la disponibilità del servizio Servizi di dominio Active Directory, in questa guida vengono fornite e consigliate istruzioni con due host Hyper-V diversi, in modo da evitare un potenziale singolo punto di errore. Non sono tuttavia necessari due host Hyper-V per eseguire la clonazione del controller di dominio virtuale.
    > -   È necessario essere membri del gruppo Administrators locale in ogni server Hyper-V (**HyperV1** e **HyperV2**).
    > -   Per importare ed esportare correttamente un file VHD utilizzando Hyper-V, i commutatori di rete virtuale in entrambi gli host Hyper-V devono avere lo stesso nome. Se, ad esempio, si dispone di un commutatore di rete virtuale in **HyperV1** denominato VNet, in **HyperV2** deve essere presente un commutatore di rete virtuale denominato VNet.
    > -   Se i due host Hyper-V (**HyperV1** e **HyperV2**) hanno processori diversi, arrestare la macchina virtuale (**VirtualDC1**) che si prevede di esportare, fare clic con il pulsante destro del mouse sulla macchina virtuale, scegliere **Impostazioni**, **Processore** e in **Compatibilità processore** selezionare **Esegui migrazione a un computer fisico con una versione diversa del processore** e quindi fare clic su **OK**.

-   Distribuito Windows Server 2012 controller di dominio (virtualizzato o fisico) che ospita il ruolo emulatore PDC (**DC1**). Per verificare se il ruolo emulatore PDC è ospitato in un controller di dominio di Windows Server 2012, eseguire il comando Windows PowerShell seguente:

    ```
    Get-ADComputer (Get-ADDomainController "Discover "Service "PrimaryDC").name "Property operatingsystemversion | fl
    ```

    Il valore di OperatingSystemVersion restituito deve essere una versione 6.2. Se necessario, è possibile trasferire il ruolo emulatore PDC al controller di dominio che esegue Windows Server 2012. Per altre informazioni, vedere [Uso dello strumento Ntdsutil.exe per assegnare o trasferire ruoli FSMO a un controller di dominio](https://support.microsoft.com/kb/255504).

-   Un controller di dominio virtualizzato guest Windows Server 2012 distribuiti (**VirtualDC1**) che è nello stesso dominio del controller di dominio Windows Server 2012 che ospita il ruolo emulatore PDC (**DC1**). Questo sarà il controller di dominio di origine utilizzato per la clonazione. Il controller di dominio virtuale guest verrà ospitato in un server Windows Server 2012 Hyper-V (**HyperV1**).

    > [!NOTE]
    > -   Perché la clonazione riesca, il controller di dominio di origine utilizzato per creare il clone non può provenire da un controller di dominio abbassato di livello dal momento della creazione del supporto VHD di origine.
    > -   Arrestare il controller di dominio di origine prima di copiare la macchina virtuale o il relativo disco rigido virtuale.
    > -   Non è consigliabile clonare un disco rigido virtuale o ripristinare uno snapshot meno recente del valore di durata per la rimozione definitiva (o del valore di durata degli oggetti eliminati se è abilitato il Cestino di Active Directory). Se si copia un disco rigido virtuale di un controller di dominio esistente, assicurarsi che il file VHD non sia meno recente del valore di durata per la rimozione definitiva (per impostazione predefinita, 60 giorni). Non è consigliabile copiare un disco rigido virtuale di un controller di dominio in esecuzione per creare il supporto di clonazione.

    Rimuovere eventuali unità disco floppy virtuali presenti nel controller di dominio di origine. Tali unità possono provocare un problema di condivisione quando si tenta di importare la nuova macchina virtuale.

    Solo controller di dominio di Windows Server 2012 ospitato in un hypervisor ID generazione macchina Virtuale è utilizzabile come origine per la clonazione. Windows Server 2012 controller di dominio utilizzato per la clonazione deve essere in uno stato integro. Per determinare lo stato del controller di dominio di origine, eseguire [dcdiag](https://technet.microsoft.com/library/cc731968(WS.10).aspx). Per ottenere una migliore comprensione dell'output restituito da dcdiag, vedere [eseguite effettivamente... si?](http://blogs.technet.com/b/askds/archive/2011/03/22/what-does-dcdiag-actually-do.aspx).

    Se il controller di dominio di origine è un server DNS, anche il controller di dominio clonato sarà un server DNS. È consigliabile scegliere un server DNS che ospita solo zone integrate in Active Directory.

    Le impostazioni del client DNS non vengono clonate, ma sono specificate nel file DCCloneConfig.xml. Se non sono specificate, il controller di dominio clonato punterà a se stesso come server DNS preferito per impostazione predefinita. Il controller di dominio clonato non disporrà di delega DNS. L'amministratore della zona DNS padre deve aggiornare la delega DNS per il controller di dominio clonato in base a quanto necessario.

    > [!WARNING]
    > Le misure di sicurezza per la virtualizzazione non si estendono ad Active Directory Lightweight Directory Services. Di conseguenza, non è consigliabile tentare di clonare un controller di dominio Servizi di dominio Active Directory che ospita un'istanza di Active Directory Lightweight Directory Services aggiungendo questa istanza al file CustomDCCloneAllowList.xml. Poiché Active Directory Lightweight Directory Services non supporta gli ID di generazione macchina virtuale, la clonazione di un controller di dominio con Active Directory Lightweight Directory Services può provocare una divergenza indotta dal rollback degli USN nel set di configurazione di Active Directory Lightweight Directory Services.

    I ruoli server indicati di seguito non sono supportati per la clonazione:

    -   Dynamic Host Configuration Protocol (DHCP)

    -   Servizi certificati Active Directory

    -   Active Directory Lightweight Directory Services

### <a name="bkmk4_grant_source"></a>Passaggio 1: Concedere al controller di dominio virtualizzato l'autorizzazione necessaria per la clonazione
In questa procedura si concede al controller di dominio di origine l'autorizzazione necessaria per la clonazione utilizzando **Centro di amministrazione di Active Directory** per aggiungere il controller di dominio di origine al gruppo **Controller di dominio clonabili**.

##### <a name="to-grant-the-source-virtualized-domain-controller-the-permission-to-be-cloned"></a>Per concedere al controller di dominio virtualizzato l'autorizzazione necessaria per la clonazione

1.  In qualsiasi controller di dominio incluso nello stesso dominio del controller di dominio preparato per la clonazione (**VirtualDC1**) aprire **Centro di amministrazione di Active Directory** , individuare l'oggetto controller di dominio virtualizzato (i controller di dominio si trovano in genere nel contenitore **Controller di dominio** in Centro di amministrazione di Active Directory), fare clic con il pulsante destro del mouse sul controller, scegliere **Aggiungi al gruppo** e in **Immettere il nome dell'oggetto da selezionare** digitare **Cloneable Domain Controllers** e quindi fare clic su **OK**.

    L'aggiornamento dell'appartenenza al gruppo eseguita in questo passaggio deve essere replicata nell'emulatore PDC prima che sia possibile procedere alla clonazione. Se il **controller di dominio clonabili** gruppo non viene trovato, il ruolo emulatore PDC potrebbe non essere ospitato in un controller di dominio che esegue Windows Server 2012.

    > [!NOTE]
    > Per aprire CENTRO in un controller di dominio di Windows Server 2012, aprire Windows PowerShell e digitare **dsac.exe**.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il cmdlet di Windows PowerShell seguente esegue la stessa funzione della procedura precedente:


    Add-ADGroupMember "Identity "CN=Cloneable Domain Controllers,CN=Users, DC=Fabrikam,DC=Com" "Member "CN=VirtualDC1,OU=Domain Controllers,DC=Fabrikam,DC=com"


### <a name="bkmk6_run_get-addccloningexcludedapplicationlist_cmdlet"></a>Passaggio 2: Eseguire il cmdlet Get-ADDCCloningExcludedApplicationList
In questa procedura viene eseguito il cmdlet `Get-ADDCCloningExcludedApplicationList` nel controller di dominio virtualizzato di origine per identificare tutti i programmi o servizi non valutati per la clonazione. È necessario eseguire il cmdlet Get-ADDCCloningExcludedApplicationList prima del cmdlet New-ADDCCloneConfigFile perché se il cmdlet New-ADDCCloneConfigFile rileva un'applicazione esclusa, non creerà un file DCCloneConfig.xml.

##### <a name="to-identify-applications-or-services-that-run-on-a-source-domain-controller-which-have-not-been-evaluated-for-cloning"></a>Per identificare le applicazioni o i servizi in esecuzione in un controller di dominio di origine e non valutati per la clonazione

1.  Nel controller di dominio di origine (**VirtualDC1**), fare clic su **Server Manager**, **Strumenti** e **Modulo di Active Directory per Windows PowerShell** e quindi digitare il comando seguente:


    Get-ADDCCloningExcludedApplicationList


2.  Esaminare insieme al fornitore software l'elenco dei servizi e dei programmi installati restituiti per determinare se possano essere clonati in modo sicuro. In caso contrario, è necessario rimuoverli dal controller di dominio di origine o la clonazione non riuscirà.

3.  Per il set di servizi e programmi installati che sono stato sviluppati per essere clonati in modo sicuro, eseguire il comando con il **"GenerateXML** switch per effettuare il provisioning di questi servizi e programmi nel **Customdccloneallowlist** file.


    Get-ADDCCloningExcludedApplicationList - GenerateXml


### <a name="bkmk5_create_insert_dccloneconfig"></a>Passaggio 3: Esecuzione di New-ADDCCloneConfigFile
Eseguire New-ADDCCloneConfigFile nel controller di dominio di origine e, facoltativamente, specificare impostazioni di configurazione per il controller di dominio clone, come il nome, l'indirizzo IP e il resolver DNS.

Per creare, ad esempio, un controller di dominio clone denominato VirtualDC2 con un indirizzo IPv4 statico, digitare:


    New-ADDCCloneConfigFile "Static -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.255.0" -CloneComputerName "VirtualDC2" -IPv4DefaultGateway "10.0.0.3" -SiteName "REDMOND"

> [!NOTE]
> Il controller di dominio clone sarà posizionato nello stesso sito del controller di dominio di origine, a meno che nel file DCCloneConfig.xml non venga specificato un sito diverso. È consigliabile specificare un sito appropriato nel file DCCloneConfig.xml per il controller di dominio clone in base al relativo indirizzo IP.

Il nome del computer è facoltativo. Se non se ne specifica uno, verrà generato un nome univoco in base all'algoritmo seguente:

-   Il prefisso è costituito dai primi otto caratteri del nome computer del controller di dominio di origine. Il nome SourceComputer di un computer di origine viene troncato in base a una stringa prefisso SourceCo.

-   Un suffisso di denominazione univoco nel formato "" CL*nnnn*"viene aggiunto alla stringa di prefisso in *nnnn* è il successivo valore disponibile da 0001 e 9999 che determina il PDC non è attualmente in uso. Se, ad esempio, 0047 è il successivo numero disponibile nell'intervallo consentito, utilizzando l'esempio precedente del prefisso del nome computer SourceCo, il nome derivato da utilizzare per il computer clone verrà impostato come SourceCo-CL0047.

> [!NOTE]
> Perché il cmdlet New-ADDCCloneConfigFile funzioni correttamente, è necessario un server di catalogo globale. L'appartenenza del controller di dominio di origine di **controller di dominio clonabili** gruppo deve riflettersi sul catalogo globale. Il catalogo globale non deve essere lo stesso controller di dominio dell'emulatore PDC, ma deve preferibilmente trovarsi nello stesso sito. Se un catalogo globale non è disponibile, il comando ha esito negativo con l'errore "il server non operativo". Per ulteriori informazioni, vedere [virtualizzato risoluzione dei problemi Controller di dominio](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).

Per creare un controller di dominio clone denominato Clone1 con impostazioni IPv4 statiche e specificare i server WINS preferito e alternativo, digitare:


    New-ADDCCloneConfigFile "CloneComputerName "Clone1" "Static -IPv4Address "10.0.0.5" "IPv4DNSResolver "10.0.0.1" "IPv4SubnetMask "255.255.0.0" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


> [!NOTE]
> Se si specificano server WINS, è necessario specificare sia **"PreferredWINSServer** e **" AlternateWINSServer**. Se si specifica solo uno di questi due argomenti ,la clonazione non riesce e viene restituito un errore con codice errore 0x80041005, visualizzato nel file dcpromo.log.

Per creare un controller di dominio clone denominato Clone2 con impostazioni IPv4 dinamiche, digitare:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" 


> [!NOTE]
> In questo caso, deve essere presente un server DHCP nell'ambiente che possa essere raggiunto dal clone e di cui il clone possa ottenere l'indirizzo IP e altre impostazioni di rete pertinenti.

Per creare un controller di dominio clone denominato Clone2 con impostazioni IPv4 statiche e specificare i server WINS preferito e alternativo, digitare:


    New-ADDCCloneConfigFile -CloneComputerName "Clone2" -IPv4DNSResolver "10.0.0.1" -SiteName "REDMOND" "PreferredWinsServer "10.0.0.1" "AlternateWinsServer "10.0.0.2"


Per creare un controller di dominio clone con impostazioni IPv6 dinamiche, digitare:


    New-ADDCCloneConfigFile -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


Per creare un controller di dominio clone con impostazioni IPv6 statiche, digitare:


    New-ADDCCloneConfigFile "Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc"


> [!NOTE]
> Quando si specificano impostazioni IPv6, l'unica differenza tra le impostazioni statiche e dinamiche è l'inclusione di **-statico** passare. L'inclusione del **-statico** commutatore rende obbligatorio specificare almeno un **IPv6DNSResolver**. L'indirizzo IPv6 statico deve essere configurata tramite la configurazione automatica indirizzo senza stato (SLAAC) con i prefissi di router assegnato. Con IPv6 dinamico, il resolver DNS sono facoltativi, ma è previsto che il clone possa raggiungere un server DHCP abilitato all'utilizzo di IPv6 nella subnet per ottenere informazioni sulla configurazione DNS e l'indirizzo IPv6.

#### <a name="BKMK_OfflineMode"></a>Esecuzione di New-ADDCCloneConfigFile in modalità offline
Se si dispone di più copie dei supporti del controller di dominio di origine preparate per la clonazione, ovvero il controller di dominio di origine è autorizzato per la clonazione, il cmdlet Get-ADDCCloningExcludedApplicationList è stato eseguito e così via, e si desidera specificare impostazioni diverse per ogni copia dei supporti, è possibile eseguire New-ADDCCloneConfigFile in modalità offline. Questo metodo può essere più efficiente della singola preparazione di ogni macchina virtuale, ad esempio, tramite l'importazione di ogni copia.

In questo caso, gli amministratori di dominio possono montare il disco offline e utilizzare strumenti di amministrazione Server remota (RSAT) per eseguire il cmdlet New-ADDCCloneConfigFile con l'argomento - offline allo scopo di aggiungere i file XML, che consente l'automazione di factory simile con nuove opzioni di Windows PowerShell incluse in Windows Server 2012. Per ulteriori informazioni su come montare il disco offline per eseguire il cmdlet New-ADDCCloneConfigFile in modalità offline, vedere la [sezione relativa all'aggiunta del file XML al disco di sistema offline](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Deployment-and-Configuration.md#BKMK_Offline).

È innanzitutto consigliabile eseguire il cmdlet localmente nel supporto di origine per garantire il superamento dei controlli dei prerequisiti. I controlli dei prerequisiti non vengono eseguiti in modalità offline, in quanto il cmdlet potrebbe essere eseguito da una macchina che non fa parte dello stesso dominio o che non è di un computer appartenente al dominio. Dopo aver eseguito il cmdlet localmente, verrà creato un file DCCloneConfig.xml. È possibile eliminare il file DCCloneConfig.xml creato localmente se successivamente si prevede di utilizzare la modalità offline.

Per creare un controller di dominio clone denominato CloneDC1 in modalità non in linea, in un sito denominato REDMOND"con indirizzo IPv4 statico, tipo:


    New-ADDCCloneConfigFile -Offline -CloneComputerName CloneDC1 -SiteName REDMOND -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -Static -Path F:\Windows\NTDS


Per creare un controller di dominio clone denominato Clone2 in modalità offline con impostazioni IPv4 statiche e IPv6 statiche, digitare:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.2" -IPv4DNSResolver "10.0.0.1" -IPv4SubnetMask "255.255.0.0" -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone2" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -Path F:\Windows\NTDS


Per creare un controller di dominio clone in modalità offline con impostazioni IPv4 statiche e IPv6 dinamiche e specificare più server DNS per le impostazioni del resolver DNS, digitare:


    New-ADDCCloneConfigFile -Offline -IPv4Address "10.0.0.10" -IPv4SubnetMask "255.255.0.0" -IPv4DefaultGateway "10.0.0.1" -IPv4DNSResolver @( "10.0.0.1","10.0.0.2" ) -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS 


Per creare un controller di dominio clone denominato Clone1 in modalità offline con impostazioni IPv4 dinamiche e IPv6 statiche, digitare:


    New-ADDCCloneConfigFile -Offline -Static -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -CloneComputerName "Clone1" -PreferredWINSServer "10.0.0.1" -AlternateWINSServer "10.0.0.3" -SiteName "REDMOND" -Path F:\Windows\NTDS


Per creare un controller di dominio clone in modalità offline con impostazioni IPv4 dinamiche e IPv6 dinamiche, digitare:


    New-ADDCCloneConfigFile -Offline -IPv4DNSResolver "10.0.0.1" -IPv6DNSResolver "2002:4898:e0:31fc:d61:2b0a:c9c9:2ccc" -Path F:\Windows\NTDS


### <a name="bkmk7_export_import_vm_sourcedc"></a>Passaggio 4: Esportare e quindi importare la macchina virtuale del controller di dominio di origine
In questa procedura viene esportata la macchina virtuale del controller di dominio virtualizzato di origine, quindi la macchina virtuale viene importata. Tramite questa azione viene creato un controller di dominio virtualizzato clone nel dominio.

È necessario essere membri del gruppo Administrators locale in ogni host Hyper-V. Se si utilizzano credenziali diverse per ogni server, eseguire i cmdlet di Windows PowerShell per esportare e importare la macchina virtuale in diverse sessioni di Windows PowerShell.

Se nel controller di dominio di origine sono presenti snapshot, devono essere eliminati prima che il controller di dominio di origine venga esportato, in quanto la macchina virtuale non verrà importata se uno snapshot include impostazioni del processore incompatibili con l'host Hyper-V di destinazione. Se le impostazioni del processore sono compatibili tra gli host Hyper-V di origine e di destinazione, è possibile esportare e copiare l'origine senza eliminare prima gli snapshot. Al termine dell'importazione, tuttavia, è necessario eliminare gli snapshot dalla macchina virtuale clone prima che questa venga avviata.

##### <a name="to-copy-a-virtual-domain-controller-by-exporting-and-then-importing-the-virtualized-source-domain-controller"></a>Per copiare un controller di dominio virtuale esportando e quindi importando il controller di dominio di origine virtualizzato

1.  In **HyperV1** arrestare il controller di dominio di origine (**VirtualDC1**).

    ![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

    Stop-VM-nome VirtualDC1 - ComputerName HyperV1


2.  In **HyperV1** eliminare gli snapshot e quindi esportare il controller di dominio di origine (VirtualDC1) nella directory c:\CloneDCs.

> [!NOTE]
> È consigliabile eliminare tutti gli snapshot associati, in quanto ogni volta che viene creato uno snapshot, viene creato anche un nuovo file AVHD che funge da disco differenze. Questo comportamento crea un effetto a catena. Se sono stati creati snapshot e si inserisce il file DCCLoneConfig.xml nel disco rigido virtuale, si rischia di creare un clone da una versione dell'albero delle informazioni di directory meno recente o di inserire il file di configurazione nel file VHD non corretto. L'eliminazione dello snapshot comporta l'unione di tutti questi file AVHD nel file VHD di base.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *


    Get-VMSnapshot VirtualDC1 | Remove-VMSnapshot -IncludeAllChildSnapshots
    Export-VM -Name VirtualDC1 -ComputerName HyperV1 -Path c:\CloneDCs\VirtualDC1


3.  Copiare la cartella **virtualdc1** nella directory c:\Import di **HyperV2**.

4.  In **HyperV2**usando **Console di gestione di Hyper-V**importare la macchina virtuale (con la procedura guidata **Importa macchina virtuale** in **Console di gestione di Hyper-V**) dalla cartella **c:\Import\virtualdc1** ed eliminare tutti gli **snapshot**associati.

Utilizzare l'opzione **Copia macchina virtuale (crea nuovo ID univoco)** quando si importa la macchina virtuale.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

    $path = Get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual Machines"
    $vm = Import-VM -Path $path.fullname -Copy -GenerateNewId
    Rename-VM $vm VirtualDC2


Per creare più controller di dominio clone dallo stesso controller di dominio di origine:

  -   Interfaccia utente: nella procedura guidata **Importa macchina virtuale** specificare nuovi percorsi per **Cartella di configurazione macchina virtuale**, **Archivio snapshot**, **Archivio file di paging intelligente**e un percorso diverso per i dischi rigidi virtuali per la macchina virtuale in **Percorso** .

  -   Windows PowerShell: specificare nuovi percorsi per la macchina virtuale utilizzando i seguenti parametri per il `Import-VM` cmdlet:

        $path = get-ChildItem "C:\CloneDCs\VirtualDC1\VirtualDC1\Virtual macchine" Import-VM-percorso $path.fullname-copia - GenerateNewId - ComputerName HyperV2 - VhdDestinationPath "path" - SnapshotFilePath "percorso" - SmartPagingFilePath "path" - VirtualMachinePath "path"


> [!NOTE]
> La dimensione batch consigliata per la creazione simultanea di più controller di dominio clone è 10. Il numero massimo è limitato dal numero massimo di connessioni di replica in uscita, che per impostazione predefinita è 16 per il servizio Replica DFS e 10 per il servizio Replica file. Evitare di distribuire simultaneamente un numero di controller di dominio clone maggiore di quello consigliato, a meno che tale numero non sia stato testato accuratamente nell'ambiente.

5.  In **HyperV1**riavviare il controller di dominio di origine (**(VirtualDC1**) per riportarlo online.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

    Start-VM -Name VirtualDC1 -ComputerName HyperV1


6.  In **HyperV2**avviare la macchina virtuale (**VirtualDC2**) per portarla online come controller di dominio clone nel dominio.

![Introduzione ad Active Directory](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *


    Start-VM -Name VirtualDC2 -ComputerName HyperV2

> [!NOTE]
> Perché la clonazione riesca, l'emulatore PDC deve essere in esecuzione. Se è stato arrestato, assicurarsi che sia stato avviato e che abbia eseguito la sincronizzazione iniziale in modo che riconosca di contenere il ruolo emulatore PDC. Per altre informazioni, vedere l' [articolo 305476 della Microsoft Knowledge Base](https://support.microsoft.com/kb/305476).

Al termine della clonazione, verificare il nome del computer clone per garantire che l'operazione di clonazione sia riuscita. Verificare che la macchina virtuale non sia stata avviata in modalità ripristino servizi directory. Se si tenta di accedere e si riceve un errore indicante che non sono disponibili server di accesso, provare ad accedere in modalità ripristino servizi directory. Se il controller di dominio non è stato clonato correttamente e viene avviato in modalità ripristino servizi directory, controllare i log nel Visualizzatore eventi e i log dcpromo nella cartella %systemroot%/debug.

Il controller di dominio clonato sarà membro del gruppo **Controller di dominio clonabili** perché copia l'appartenenza dal controller di dominio di origine. Una procedura consigliata consiste nel lasciare vuoto il gruppo **Controller di dominio clonabili** fino a quando non si è pronti per eseguire le operazioni di clonazione e nel rimuovere i membri al termine delle operazioni di clonazione.

Se nel controller di dominio di origine è archiviato un supporto di backup, questo sarà archiviato anche nel controller di dominio clonato. È possibile eseguire `wbadmin get versions` per mostrare il supporto di backup nel controller di dominio clonato. Un membro del gruppo Domain Admins deve eliminare il supporto di backup nel controller di dominio clonato per impedirne il ripristino accidentale. Per ulteriori informazioni su come eliminare un backup dello stato del sistema mediante wbadmin.exe, vedere [Wbadmin delete systemstatebackup](https://technet.microsoft.com/library/cc742081(v=WS.10).aspx).

## <a name="troubleshooting"></a>Risoluzione dei problemi
Se il controller di dominio clone (**VirtualDC2**) viene avviato in modalità ripristino servizi directory, non torna automaticamente in una modalità normale al successivo riavvio. Per accedere a un controller di dominio avviato in modalità ripristino servizi directory, utilizzare **.\Administrator** e specificare la password della modalità ripristino servizi directory.

Correggere la causa dell'errore di clonazione e verificare che il log dcpromo.log non indichi che non è possibile riprovare la clonazione. Se non è possibile riprovare la clonazione, sostituire il supporto in modo sicuro. Se è possibile riprovare la clonazione, è necessario rimuovere il flag di avvio DSRM per riprovare la clonazione.

1.  Aprire Windows Server 2012 con un comando con privilegi elevati (a destra fare clic su Windows Server 2012 e scegliere Esegui come amministratore), quindi digitare **msconfig**.

2.  Nella scheda **Avvio** deselezionare **Modalità provvisoria**in **Opzioni di avvio** . Questa opzione è già selezionata con l'opzione **Ripristina Active Directory**abilitata.

3.  Fare clic su **OK** e riavviare quando viene richiesto.

Per ulteriori informazioni sulla risoluzione dei problemi, vedere l'[articolo sulla risoluzione dei problemi relativi ai controller di dominio virtualizzati](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).


