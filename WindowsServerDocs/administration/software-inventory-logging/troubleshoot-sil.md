---
title: Risoluzione dei problemi relativi a Registrazione inventario software
description: Viene descritto come risolvere i problemi di distribuzione comuni funzionalità registrazione inventario Software.
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
ms.openlocfilehash: 6ea8a336e2c40b55e2ad6b508d89db7dcb668315
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832852"
---
# <a name="troubleshoot-software-inventory-logging"></a>Risoluzione dei problemi relativi a Registrazione inventario software 

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

##<a name="understanding-sil"></a>Informazioni su registrazione inventario software

Prima di iniziare la risoluzione dei problemi di registrazione inventario software, si deve avere una buona conoscenza dei suoi componenti e il relativo funzionamento. I video seguenti offrono una panoramica di registrazione inventario software e SIL Aggregator e come usarli per l'inoltro e i dati di inventario di report:

1. [Un'introduzione a registrazione inventario Software (SIL) (10:57)](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [Software di registrazione inventario: Configurazione di SIL Aggregator (14:34)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [Software di registrazione inventario: Enabling SIL Forwarding (7:20)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

##<a name="how-sil-data-flow-works"></a>Modo in cui i dati SIL passare works

Il framework di registrazione inventario software ha due componenti principali e due canali di comunicazione. Flusso di dati su entrambi i canali e tra entrambi i componenti, è necessario per una corretta distribuzione di registrazione inventario software (ciò presuppone un ambiente virtualizzato o cloud, - ambienti fisici puramente sufficiente solo uno dei canali di comunicazione). È necessario comprendere i componenti e flusso di dati di registrazione inventario software per distribuirlo in modo corretto. Dopo aver guardato i video di panoramica precedenti, si sarà viste diagramma dell'architettura in cui vengono illustrati i componenti e flusso di dati su entrambi i canali. Le frecce di colore arancione indicano le query remote tramite WinRM, frecce tratteggiate verde indicano che HTTPS post a SIL Aggregator di registrazione inventario software in ogni nodo di fine WS:

![](../media/software-inventory-logging/image1.png)

Se si verifica un problema con registrazione inventario software, è probabilmente correlato a un'interruzione del flusso di dati tramite i canali e tra i componenti. Di seguito sono i problemi più comuni correlati al flusso di dati, quindi nella sezione successiva per la risoluzione dei problemi da risolvere ognuno dei tre problemi:

-   **Problema 1 del flusso di dati** – **alcun dato nel report quando si usa il cmdlet Publish-SilReport** (o a livello generale i dati mancanti).

-   **Problema 2 del flusso di dati** – **eccessivo numero di server nell'Host sconosciuto** nel report.

-   **Problema 3 del flusso di dati** – **eccessivo numero di macchine virtuali in host fisici elencato come sistema operativo sconosciuto** nel report e/o un errore generato quando si usa **Publish-SilData** nei server Windows in esecuzione registrazione inventario software.

##<a name="troubleshooting-data-flow-issues"></a>Risoluzione dei problemi di flusso di dati

Prima di iniziare, è necessario sapere quanto tempo prima SIL Aggregator iniziare a usare il **Start-SilAggregator** cmdlet.

>[!IMPORTANT]
>Finché non verrà senza dati nel report viene elaborato il cubo di dati SQL in fase di sistema locale 3 AM. Non procedere con la procedura di risoluzione fino a quando il cubo ha elaborato i dati.

Se si siano risolvendo i dati nel report (o mancante dal report) più recenti rispetto all'ultima ora o il cubo elaborato prima che il cubo sia elaborato mai (per una nuova installazione), seguire questi passaggi per elaborare il cubo di dati SQL in tempo reale :

1. Accedere come amministratore di SQL Server ed eseguire **SSMS** un prompt dei comandi.
2. Connettersi al motore di database.
3. Espandere **SQL Server Agent** e quindi espandere **processi**.
4. Fare clic destro **SILStagingRefresh** e quindi selezionare **inizia processo al passaggio**.
5. Fare clic su **avviare** e attendere che l'indicatore di stato di aggiornamento per il completamento.
6. Aprire PowerShell come amministratore ed eseguire la **silreport pubblicare - ApriReport** cmdlet.

Se non sono ancora presenti dati nel report, procedere alla risoluzione dei problemi di flusso di tre dati.

###<a name="data-flow-issue-1"></a>Problema di flusso di dati 1 

####<a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>Nessun dato del report quando si usa il cmdlet Publish-SilReport (o mancano i dati a livello generale)

Se mancano i dati, è probabile che a causa di un cubo dei dati di SQL non avere ancora elaborata. Se è stato elaborato di recente e si ritiene che i dati mancanti devono succedere in Sil Aggregator prima dell'elaborazione del cubo, seguire il percorso dei dati in ordine inverso. Selezionare un host univoco e una macchina virtuale univoca per risolvere i problemi. Il percorso dei dati in ordine inverso sarebbe **Report SILA** &lt; **database SILA** &lt; **directory locale SILA** &lt;  **host fisico remoto** oppure **WS VM che eseguono attività di registrazione inventario softwareagente/**.

####<a name="check-to-see-if-data-is-in-the-database"></a>Selezionare questa opzione per verificare se i dati nel database

Esistono due modi per verificare la presenza di dati: **PowerShell** oppure **SSMS**.

>[!Important]
>Se il cubo elaborato almeno una volta poiché SILA inseriti i dati nel database di, questi dati devono essere visibili nel report. Se non sono presenti dati nel database quindi entrambi gli host fisici il polling non riesce o non vengono ricevuti tramite HTTPS o entrambi.

 ####<a name="powershell"></a>PowerShell

1. Aprire PowerShell come amministratore ed eseguire la **get-silvmhost** cmdlet, quindi eseguire **get-silaggregator**.

    >[!NOTE]
    >L'output del **get-silaggregator** sempre imitino il **scheda dettagli di Windows Server** del rapporto di Excel. Non prevede un risultato diverso.

2. Eseguire **get-silvmhost**
 - Se non esiste nessun host non è elencato, quindi aggiungere gli host che utilizzano le **aggiungere-silvmhost** cmdlet.
 - Se gli host non sono elencati come **sconosciuto**, quindi passare al problema 2. d: se sono elencati gli host, ma è presente alcun **datetime** sotto il **recente sondaggio** colonna, quindi passare a **problema 2** sotto.

**Altri comandi correlati**

**Get-SilAggregator - Computername &lt;fqdn di un server è noto il push dei dati&gt;**: Informazioni dal database su un computer (VM) si otterrà anche prima che il cubo sia elaborato. Pertanto questo cmdlet è utilizzabile per controllare i dati nel database per un Server di Windows il push dei dati di registrazione inventario software tramite HTTPS, prima o senza, il processo del cubo alle 3:00 (o se ancora stato aggiornato il cubo in tempo reale come descritto all'inizio di questa sezione).

**Get-SilAggregator - VmHostName &lt;fqdn del polling di un host fisico in cui è presente un valore nella colonna polling recenti quando si usa il cmdlet Get-SilVmHost&gt;**: Informazioni dal database su un host fisico si otterrà anche prima che il cubo sia elaborato.

####<a name="ssms"></a>SSMS

n**verificare la presenza di dati dagli host viene eseguito il polling:**
 
1. Aprire **SSMS** e connettersi al **motore di Database**.
2. Espandere **database**, espandere il **SoftwareInventoryLogging** del database, espandere **tabelle**, fare clic il **HostInfo** tabella e quindi Seleziona le prime 1000 righe. 

    Se sono presenti dati per uno o più host nella tabella, quindi il polling di host o negli host che/quelli è stata eseguita correttamente almeno una volta.

 **Verificare i dati da server autonomi, che hanno effettuato il push dei dati tramite HTTPS, o macchine virtuali:** 

1. Aprire **SSMS** e connettersi al **motore di Database**.
a2. Espandere **database**, espandere il **SoftwareInventoryLogging** del database, espandere **tabelle**, fare clic il **VMInfo** tabella e quindi Seleziona le prime 1000 righe. 

    >[!NOTE] 
    >Ogni riga per una macchina virtuale univoca rappresenterà uno elaborati **bmil** file è stato effettuato il push in HTTPS ed elaborato da SIL Aggregator. File Bmil sono presenti file proprietari usati da SIL, ne viene creata ogni nostro per ogni istanza di registrazione inventario software, si noti che questo è solo necessario quando SIL e SILA vengono usate negli ambienti virtuali. In caso contrario, solo il traffico HTTPS non è necessario/previsto).

 Tutti i dati nel database devono essere riflessa in report di registrazione inventario software dopo che ha elaborato il cubo.

###<a name="data-flow-issue-2"></a>Problema di flusso di dati 2
####<a name="too-many-servers-under-unknown-host"></a>Troppi i server Host sconosciuto

Questo problema può verificarsi in ambienti virtuali quando SIL Aggregator non esegue il polling host fisici che ospitano le macchine virtuali.

1. Aprire **PowerShell** come amministratore ed eseguire il **get-silvmhost** cmdlet.

    -   Se gli host non sono elencati come **sconosciuto**, il **aggiungere-silvmhost** cmdlet non funzionavano correttamente: in genere a causa di credenziali errate aggiunte per l'accesso a questi host (in questo modo, sconosciuto). Ma se vengono verificate le credenziali, è possibile che il rilevamento automatico delle **hosttype** e **hypervisortype** nel **aggiungere-silvmhost** cmdlet non è in grado di riconoscere il piattaforma. Vi sono avanzati risoluzione dei problemi disponibili per queste situazioni, ma non sono trattati qui (verificare i canali EventViewer SIL Aggregator).

    -   Se sono elencati gli host, e **hosttype** e **hypervisortype** sono elencati con valori diversi da quelli **sconosciuto**, ovvero Windows e Hyper-v, o Ubuntu e Xen, e così via, ma non esiste alcuna **data/ora** sotto **recente sondaggio** colonna, quindi il polling non è stato verificato ancora.

        -   Sarà necessario attendere un'ora dopo l'aggiunta di host per il polling da eseguire (presupponendo che questo intervallo è impostato sul valore predefinito: può essere controllato tramite il **get-silaggregator** cmdlet).

        -   Se è stato un'ora perché è stato aggiunto l'host, verificare che l'attività di polling è in esecuzione: Nelle **utilità di pianificazione**, selezionare **Software Inventory Logging Aggregator** sotto **Microsoft** &gt; **Windows** e controllare la cronologia dell'attività.

    -   Se un host verrà elencato, ma è presente alcun valore per **RecentPoll**, **HostType**, o **HypervisorType**, ciò può essere ignorata. Ciò si verifica solo in ambienti Hyper-v. Questi dati provengono effettivamente dalla VM Windows Server, che identifica l'host fisico che è in esecuzione su HTTPS. Ciò può essere utile per identificare una determinata macchina virtuale che invia report, ma richiede che il data mining del database utilizzando la **Get-SilAggregatorData** cmdlet.

Una volta che gli host eseguono il polling in modo corretto, sarà possibile visualizzare i dati per tali host fisico nel database SILA in cui è presente una **data/ora** sotto **recente sondaggio**. Problema 1 sezione precedente fornisce istruzioni per il recupero di tali dati.

###<a name="data-flow-issue-3"></a>Problema di flusso di dati 3
####<a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>Troppi fisico host con macchine virtuali elencate come sistema operativo sconosciuto

1. Selezionare un nodo Windows Server end (VM) che già conosci è su uno di questi host, effettuare l'accesso come amministratore.
2. Aprire PowerShell come amministratore.
3. Verificare **SilLogging** è in esecuzione usando la **Get-SilLogging** cmdlet.
 - Se in esecuzione, provare a effettuare manualmente il push dei dati SIL tramite **Publish-SilData**.

  - Se si verifica un errore:
     - Verificare che il **targeturi** ha **https://** nella voce.
     - Assicurarsi che siano soddisfatti tutti i prerequisiti 
     - Verificare che siano installati tutti gli aggiornamenti necessari per Windows Server (vedere i prerequisiti per registrazione inventario software). Un modo rapido per verificare (in WS 2012 R2 solo) viene possibile ricercarli usando il cmdlet seguente: **Get-SilWindowsUpdate \*3060, \*3000**
     - Verificare il certificato usato per autenticare Sil Aggregator sia installato nel percorso corretto nel server locale deve essere eseguito l'inventario con **SilLogging**.
     - In SIL Aggregator, assicurarsi che l'identificazione personale del certificato utilizzato per eseguire l'autenticazione con l'aggregatore viene aggiunto all'elenco utilizzando il **Set-SilAggregator** **-AddCertificateThumbprint** cmdlet.
     - Se si usano certificati dell’organizzazione, assicurarsi che il server in cui è stata abilitata la funzionalità Registrazione inventario software appartenga al dominio per il quale è stato creato il certificato o sia in altro modo verificabile tramite un’autorità radice. Se un certificato non è attendibile nel computer locale che sta tentando di eseguire l’inoltro o il push dei dati a SIL Aggregator, l’azione viene interrotta con un errore.

    Se il problema è stato archiviato e verificata, ma il problema persiste:

    - Verificare che il certificato usato per l'installazione di SIL Aggregator sia integro e corrisponde al nome del server di SIL Aggregator stesso. Inoltre, se si usano certificati aziendali per SIL Aggregator, potrebbe essere necessario aggiungere SIL Aggregator al dominio in cui è stato creato il certificato. Questa procedura non è necessaria se altri computer inoltrano correttamente allo stesso SIL Aggregator.

    -  Infine, è possibile controllare il percorso per i file di registrazione inventario software memorizzati nella cache sul server che tenta di eseguire l'inoltro/push, **\Windows\System32\Logfiles\SIL**. Se **SilLogging** è stata avviata ed è in esecuzione per più di un'ora, o **Publish-SilData** è stata eseguita di recente, e non sono presenti file in questa directory, la registrazione Sil Aggregator ITaaS esito positivo.

Se è presente alcun errore e alcun output nella console, quindi i dati push/pubblicazione dal nodo finale di Windows Server a SIL Aggregator tramite HTTPS è riuscita. Per seguire il percorso dell'account di accesso in avanti, i dati a SIL Aggregator come amministratore ed esaminare i file di dati che sono stati ricevuti. Passare a **Program Files (x86)** &gt; **Microsoft SIL Aggregator** &gt; directory SILA. È possibile controllare i file di dati che arrivano in tempo reale.

>[!NOTE] 
>Più di un file di dati sono stato trasferito con il **Publish-SilData** cmdlet. Registrazione inventario software nel nodo di fine verrà memorizzati nella cache di push non riusciti fino a 30 giorni. Il push successivo ha esito positivo verranno inviata tutti i file di dati a Sil Aggregator per l'elaborazione. In questo modo, nuova impostazione di SIL Aggregator è stato possibile visualizzare i dati da un nodo finale molto prima di un proprio programma di installazione.

>[!NOTE] 
>Sono presenti regole che SILA segue durante l'elaborazione dei file di dati nella directory SILA rilevanti solo in situazioni di traffico ridotto. Traffico elevato verrà sempre attivata l'elaborazione in tempo reale. Il comportamento predefinito prevede che l'elaborazione inizierà uno dopo l'arrivo dei 100 file nella directory, o dopo 15 minuti. La risoluzione dei problemi end-to-end in un ambiente di piccole dimensioni, è spesso necessario attendere 15 minuti.

Dopo che vengono elaborati, si visualizzeranno i dati nel database.
