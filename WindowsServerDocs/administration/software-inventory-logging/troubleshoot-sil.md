---
title: Risoluzione dei problemi relativi a Registrazione inventario software
description: Viene descritto come risolvere i problemi comuni relativi alla distribuzione di registrazione inventario software.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
author: brentfor
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 1878347234affddcb7a7acb39c532712051ea223
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866267"
---
# <a name="troubleshoot-software-inventory-logging"></a>Risoluzione dei problemi relativi a Registrazione inventario software 

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

## <a name="understanding-sil"></a>Informazioni su SIL

Prima di iniziare la risoluzione dei problemi di registrazione inventario software, è necessario avere una conoscenza ottimale dei relativi componenti e del relativo funzionamento. I video seguenti forniscono una panoramica di SIL e SIL aggregator e come usarli per inviare e segnalare i dati di inventario:

1. [Introduzione a registrazione inventario software (SIL) (10:57)](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [Registrazione inventario software: Configurazione di SIL Aggregator (14:34)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [Registrazione inventario software: Abilitazione dell'invio SIL (7:20)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

## <a name="how-sil-data-flow-works"></a>Funzionamento del flusso di dati SIL

SIL Framework include due componenti principali e due canali di comunicazione. Il flusso di dati su entrambi i canali e tra entrambi i componenti è necessario per una distribuzione SIL corretta (si presuppone che un ambiente virtualizzato, o cloud, sia un ambiente puramente fisico che necessita solo di uno dei canali di comunicazione). È necessario comprendere i componenti e il flusso di dati di SIL per distribuirlo correttamente. Dopo aver guardato i video introduttivi sopra indicati, si sarà visto il diagramma dell'architettura che illustra i componenti e il flusso di dati su entrambi i canali. Le frecce arancioni indicano le query remote su WinRM, le frecce verdi tratteggiate indicano i post HTTPS per SIL aggregator da SIL in ogni nodo WS-end:

![](../media/software-inventory-logging/image1.png)

Se si verifica un problema con registrazione inventario software, è probabile che si verifichi un'interferenza nel flusso di dati sui canali e tra i componenti. Di seguito sono riportati i problemi più comuni relativi al flusso di dati, seguiti nella sezione successiva dalla procedura di risoluzione dei problemi per risolvere ognuno dei tre problemi:

-   **Problema del flusso di dati 1** : **Nessun dato nel report quando si usa il cmdlet Publish-SilReport** (o in genere mancano i dati).

-   **Problema del flusso di dati 2** : **troppi server nell'host sconosciuto** del report.

-   **Problema del flusso di dati 3** : **troppe macchine virtuali negli host fisici elencati come sistemi operativi sconosciuti** nel report e/o un errore generato quando si usa **Publish-SilData** nei server Windows che eseguono SIL.

## <a name="troubleshooting-data-flow-issues"></a>Risoluzione dei problemi del flusso di dati

Prima di iniziare, è necessario essere a conoscenza del tempo prima che SIL aggregator iniziasse con il cmdlet **Start-SilAggregator** .

>[!IMPORTANT]
>Non saranno presenti dati nel report fino a quando il cubo di dati SQL non viene elaborato alle 3.00 ora di sistema locale. Non procedere con i passaggi di risoluzione dei problemi fino a quando il cubo non ha elaborato i dati.

Per risolvere i problemi relativi ai dati del report (o mancanti dal report) più recenti dell'ultima volta in cui il cubo è stato elaborato o prima dell'elaborazione del cubo (per una nuova installazione), attenersi alla procedura seguente per elaborare il cubo di dati SQL in tempo reale :

1. Accedere come amministratore di SQL Server ed eseguire **SSMS** al prompt dei comandi.
2. Connettersi al motore di database.
3. Espandere **SQL Server Agent** , quindi espandere **processi**.
4. Fare clic con il pulsante destro del mouse su **SILStagingRefresh** e quindi scegliere **Avvia processo al passaggio**.
5. Fare clic su **Start** e attendere il completamento dell'indicatore di stato dell'aggiornamento.
6. Aprire PowerShell come amministratore ed eseguire il cmdlet **Publish-silreport-ApriReport** .

Se non sono ancora presenti dati nel report, procedere con la risoluzione dei tre problemi del flusso di dati.

### <a name="data-flow-issue-1"></a>Emissione flusso di dati 1 

#### <a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>Non sono presenti dati nel report quando si usa il cmdlet Publish-SilReport (o i dati sono in genere mancanti)

Se i dati risultano mancanti, è probabile che il cubo di dati SQL non sia ancora stato elaborato. Se è stata elaborata di recente e si ritiene che i dati mancanti siano arrivati all'aggregatore prima dell'elaborazione del cubo, seguire il percorso dei dati in ordine inverso. Selezionare un host univoco e una macchina virtuale univoca per la risoluzione dei problemi. Il percorso dati in ordine inverso **è** &lt; la Sila del **database** &lt; Sila **directory** &lt; locale **host fisico remoto** o **WS VM running SIL Agent/Task**.

#### <a name="check-to-see-if-data-is-in-the-database"></a>Verificare se i dati sono nel database

Esistono due modi per verificare i dati: **PowerShell** o **SSMS**.

>[!Important]
>Se il cubo ha elaborato almeno una volta dopo che SILA ha inserito i dati nel database, tali dati devono essere riflessi nel report. Se nel database non sono presenti dati, il polling degli host fisici ha esito negativo o non viene ricevuto alcun risultato tramite HTTPS o entrambi.

 #### <a name="powershell"></a>PowerShell

1. Aprire PowerShell come amministratore ed eseguire il cmdlet **Get-silvmhost** , quindi eseguire **Get-silaggregator**.

    >[!NOTE]
    >L'output di **Get-silaggregator** simula sempre la **Scheda dettagli di Windows Server** del rapporto di Excel. Non è previsto un risultato diverso.

2. Eseguire **Get-silvmhost**
   - Se non sono elencati host, aggiungere gli host usando il cmdlet **Add-silvmhost** .
   - Se gli host sono elencati come **sconosciuti**, passare a Issue 2. 
   d: se gli host sono elencati ma non è **presente alcuna data/ora nella colonna** di **polling recente** , passare al **problema 2** riportato di seguito.

**Altri comandi correlati**

**Get-SilAggregator-ComputerName &lt;FQDN di un server noto che esegue&gt;il push dei dati**: Questa operazione produrrà informazioni dal database relative a un computer (VM) anche prima dell'elaborazione del cubo. Questo cmdlet può quindi essere usato per controllare i dati nel database per un server Windows che esegue il push dei dati SIL su HTTPS, prima o senza, il processo del cubo alle 3.00 (o se il cubo non è stato aggiornato in tempo reale come descritto all'inizio di questa sezione).

**Get-SilAggregator-VmHostName &lt;FQDN di un host fisico sottoposto a polling in cui è presente un valore nella colonna di polling recente quando si&gt;usa il cmdlet Get-SilVmHost**: In questo modo, le informazioni del database relative a un host fisico vengono generate anche prima dell'elaborazione del cubo.

#### <a name="ssms"></a>SSMS

n**verificare la presenza di dati dagli host di cui è in corso il polling:**
 
1. Aprire **SSMS** e connettersi al **motore di database**.
2. Espandere **database**, espandere il database **SoftwareInventoryLogging** , espandere **tabelle**, fare clic con il pulsante destro del mouse sulla tabella **HostInfo** , quindi selezionare le prime 1000 righe. 

    Se sono presenti dati per uno o più host nella tabella, il polling di tale host/i è riuscito almeno una volta.

   **Verificare la presenza di dati da macchine virtuali o server autonomi che hanno eseguito il push dei dati su HTTPS:** 

3. Aprire **SSMS** e connettersi al **motore di database**.
   a2. Espandere **database**, espandere il database **SoftwareInventoryLogging** , espandere **tabelle**, fare clic con il pulsante destro del mouse sulla tabella **VMInfo** , quindi selezionare le prime 1000 righe. 

    >[!NOTE] 
    >Ogni riga di una macchina virtuale univoca rappresenterà un file **bmil** elaborato che ha eseguito il push su HTTPS ed elaborato da SIL Aggregator. I file Bmil sono file proprietari usati da SIL, ognuno dei quali viene creato da ogni istanza di registrazione inventario software. si noti che questa operazione è necessaria solo quando si usano SIL e SILA in ambienti virtuali. In caso contrario, è necessario o previsto solo il traffico HTTPS.

   Tutti i dati nel database devono essere riflessi nei report SIL dopo l'elaborazione del cubo.

### <a name="data-flow-issue-2"></a>Emissione flusso di dati 2
#### <a name="too-many-servers-under-unknown-host"></a>Troppi server nell'host sconosciuto

Questa situazione si verifica probabilmente negli ambienti virtuali quando SIL aggregator non esegue il polling degli host fisici che ospitano le macchine virtuali.

1. Aprire **PowerShell** come amministratore ed eseguire il cmdlet **Get-silvmhost** .

    -   Se gli host sono elencati come **sconosciuti**, il cmdlet **Add-silvmhost** non funziona correttamente, in genere a causa di credenziali non valide aggiunte per l'accesso a questi host (pertanto, Unknown). Tuttavia, se le credenziali vengono verificate, potrebbe significare che il rilevamento automatico di **HostType** e **hypervisortype** nel cmdlet **Add-silvmhost** non è stato in grado di riconoscere la piattaforma. Per queste situazioni sono disponibili procedure avanzate per la risoluzione dei problemi, ma non sono descritte in questa pagina (controllare i canali di EventViewer SIL Aggregator).

    -   Se gli host sono elencati e **HostType** e **hypervisortype** sono elencati con valori non **sconosciuti**, ovvero Windows e HyperV, Ubuntu e Xen e così via, **ma non è presente alcuna data** /ora in una colonna di **polling recente** , quindi il polling non è stato ancora eseguito.

        -   È necessario attendere un'ora dopo l'aggiunta dell'host per l'esecuzione del polling (presupponendo che questo intervallo sia impostato su default), è possibile controllare usando il cmdlet **Get-silaggregator** .

        -   Se è stata rilevata un'ora dall'aggiunta dell'host, controllare che l'attività di polling sia in esecuzione: In **utilità di pianificazione**selezionare **Software Inventory Logging aggregator** in **Microsoft** &gt; **Windows** e verificare la cronologia dell'attività.

    -   Se è elencato un host, ma non è presente alcun valore per **RecentPoll**, **HostType**o **HypervisorType**, questo può essere ampiamente ignorato. Questa situazione si verificherà solo negli ambienti HyperV. Questi dati provengono dalla macchina virtuale Windows Server, identificando l'host fisico in esecuzione su HTTPS. Questo può essere utile per identificare una macchina virtuale specifica che sta segnalando, ma richiede il data mining del database usando il cmdlet **Get-SilAggregatorData** .

Una volta eseguito il polling degli host correttamente, sarà possibile visualizzare i dati per questi host fisici nel database SILA in cui è presente un valore **DateTime** in **polling recente**. La sezione relativa al problema 1 illustra i passaggi per il recupero dei dati.

### <a name="data-flow-issue-3"></a>Problema del flusso di dati 3
#### <a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>Troppi host fisici con macchine virtuali elencate come sistemi operativi sconosciuti

1. Selezionare un nodo di Windows Server end (VM) noto in uno di questi host, quindi accedere come amministratore.
2. Aprire PowerShell come amministratore.
3. Verificare che **SilLogging** sia in esecuzione usando il cmdlet **Get-SilLogging** .
   - Se è in esecuzione, provare a eseguire manualmente il push dei dati SIL usando **Publish-SilData**.

   - Se si verifica un errore:
     - Verificare che **targetUri** abbia **https://** nella voce.
     - Verificare che tutti i prerequisiti siano soddisfatti 
     - Verificare che tutti gli aggiornamenti necessari per Windows Server siano installati (vedere Prerequisiti per SIL). Un modo rapido per verificare (solo su WS 2012 R2) consiste nel cercarli usando il cmdlet seguente: **Get-SilWindowsUpdate \*3060, \*3000**
     - Verificare che il certificato usato per l'autenticazione con l'aggregatore sia installato nell'archivio corretto del server locale per l'inventario con **SilLogging**.
     - In SIL aggregator assicurarsi che l'identificazione personale del certificato usato per l'autenticazione con l'aggregatore venga aggiunta all'elenco usando il cmdlet **set-SilAggregator** **– AddCertificateThumbprint** .
     - Se si usano certificati dell’organizzazione, assicurarsi che il server in cui è stata abilitata la funzionalità Registrazione inventario software appartenga al dominio per il quale è stato creato il certificato o sia in altro modo verificabile tramite un’autorità radice. Se un certificato non è attendibile nel computer locale che sta tentando di eseguire l’inoltro o il push dei dati a SIL Aggregator, l’azione viene interrotta con un errore.

     Se tutti i precedenti sono stati controllati e verificati, ma il problema persiste:

     - Verificare che il certificato usato per l'installazione di SIL aggregator sia integro e corrisponda al nome del server SIL Aggregator. Inoltre, se si usano certificati aziendali per SIL Aggregator, potrebbe essere necessario aggiungere SIL Aggregator al dominio in cui è stato creato il certificato. Questa procedura non è necessaria se altri computer inoltrano correttamente allo stesso SIL Aggregator.

     -  Infine, è possibile controllare il percorso seguente per i file SIL memorizzati nella cache nel server che tenta di inoltrare/effettuare il push, **\Windows\System32\Logfiles\SIL**. Se **SilLogging** è stato avviato ed è stato eseguito per più di un'ora oppure **Publish-SilData** è stato eseguito di recente e non sono presenti file in questa directory, è possibile che la registrazione a aggregator abbia avuto esito positivo.

Se non viene generato alcun errore e non viene restituito alcun output nella console, il push o la pubblicazione di dati dal nodo end di Windows Server a SIL aggregator su HTTPS ha avuto esito positivo. Per seguire il percorso dei dati, accedere a SIL Aggregator come amministratore ed esaminare i file di dati ricevuti. Passare a **programmi (x86)** &gt; directory Sila di **Microsoft SIL aggregator** &gt; . È possibile controllare i file di dati in arrivo in tempo reale.

>[!NOTE] 
>È possibile che sia stato trasferito più di un file di dati con il cmdlet **Publish-SilData** . Registrazione inventario software nel nodo finale memorizza nella cache i push non riusciti per un massimo di 30 giorni. Al successivo push riuscito tutti i file di dati verranno indirizzati a aggregator per l'elaborazione. In questo modo, una nuova configurazione di SIL aggregator potrebbe visualizzare i dati di un nodo finale prima della propria configurazione.

>[!NOTE] 
>Quando si elaborano i file di dati nella directory SILA, le regole sono rilevanti solo in situazioni di traffico ridotto. Il traffico elevato attiverà sempre l'elaborazione in tempo reale. Il comportamento predefinito prevede che l'elaborazione venga avviata dopo l'arrivo di 100 file nella directory o dopo 15 minuti. Quando si esegue la risoluzione dei problemi end-to-end in un ambiente di piccole dimensioni, è spesso necessario attendere 15 minuti.

Una volta elaborati questi file, verranno visualizzati i dati nel database.
