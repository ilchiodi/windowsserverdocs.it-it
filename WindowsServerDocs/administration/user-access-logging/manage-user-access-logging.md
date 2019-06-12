---
title: Gestire Registrazione accesso utenti
description: Viene descritto come gestire registrazione accesso utenti
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-user-access-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f039017-4152-47eb-838e-bb6ef730b638
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03bad9864f81cf75be13b4ca391fdcbc5f9dcb5c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435352"
---
# <a name="manage-user-access-logging"></a>Gestire Registrazione accesso utenti

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo documento illustra come gestire Registrazione accesso utenti.  
  
Registrazione accesso utenti è una funzionalità che gli amministratori del server possono usare per quantificare il numero di richieste client univoche di ruoli e servizi in un server locale.  
  
La funzionalità Registrazione accesso utenti viene installata e abilitata per impostazione predefinita e raccoglie i dati in tempo quasi reale. Per Registrazione accesso utenti è disponibile un numero limitato di opzioni di configurazione. In questo documento tali opzioni vengono descritte insieme agli scopi previsti.  
  
Per altre informazioni sui vantaggi della registrazione accesso utenti, vedere la [Introduzione a registrazione accesso utenti](get-started-with-user-access-logging.md).  
  
**In questo documento**  
  
Le opzioni di configurazione descritte in questo documento includono:  
  
-   Disabilitazione e abilitazione del servizio Registrazione accesso utenti  
  
-   Raccolta e rimozione dei dati  
  
-   Eliminazione dei dati registrati da Registrazione accesso utenti  
  
-   Gestione di Registrazione accesso utenti in ambienti con volumi elevati  
  
-   Ripristino da uno stato di danneggiamento  
  
-   Abilitare la verifica delle licenze per l'utilizzo di Cartelle di lavoro   
  
## <a name="BKMK_Step1"></a>La disabilitazione e abilitazione del servizio registrazione accesso utenti  
Registrazione accesso utenti è abilitato e viene eseguito per impostazione predefinita quando un computer che esegue Windows Server 2012, o in un secondo momento, sono installato e avviato per la prima volta. Gli amministratori potrebbero avere la necessità di disattivare e disabilitare Registrazione accesso utenti per rispettare requisiti di privacy specifici o soddisfare altre esigenze operative. È possibile disattivare registrazione accesso utenti tramite la console servizi, dalla riga di comando o tramite i cmdlet di PowerShell. Tuttavia, per assicurarsi che registrazione accesso utenti non viene eseguito nuovamente al successivo che avvio del computer, è inoltre necessario disabilitare il servizio. Le procedure seguenti viene descritto come disattivare e disabilitare registrazione accesso utenti.  
  
> [!NOTE]  
> È possibile usare il cmdlet di PowerShell `Get-Service UALSVC` per recuperare informazioni sul servizio Registrazione accesso utenti e determinare tra l'altro se il servizio è in esecuzione o arrestato e se è abilitato o disabilitato.  
  
#### <a name="to-stop-and-disable-the-ual-service-by-using-the-services-console"></a>Per arrestare e disabilitare il servizio Registrazione accesso utenti tramite la console Servizi  
  
1.  Accedere al server con un account con privilegi di amministratore locale.  
  
2.  In Server Manager posizionare il puntatore su **Strumenti** e quindi fare clic su **Servizi**.  
  
3.  Scorrere verso il basso e selezionare **Servizio registrazione accessi utente**. Fare clic su **Arrestare il servizio**.  
  
4.  A destra\-fare clic sul nome del servizio e selezionare **proprietà**. Nella scheda **Generale** impostare **Tipo di avvio** su **Disabilitato**e quindi fare clic su **OK**.  
  
#### <a name="to-stop-and-disable-ual-from-the-command-line"></a>Per arrestare e disabilitare Registrazione accesso utenti dalla riga di comando  
  
1.  Accedere al server con un account con privilegi di amministratore locale.  
  
2.  Premere il tasto logo Windows + R e quindi digitare **cmd** per aprire una finestra del prompt dei comandi.  
  
    > [!IMPORTANT]  
    > Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
3.  Digitare **net stop ualsvc**, quindi premere INVIO.  
  
4.  Digitare **netsh ualsvc set opmode mode=disable**, quindi premere INVIO.  
   
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  

È inoltre possibile arrestare e disabilitare Registrazione accesso utenti tramite i comandi di Windows PowerShell Stop-service e Disable-Ual.  
  
```  
Stop-service ualsvc  
```  
  
```  
Disable-ual  
```  
  
Se in un secondo momento si desidera riavviare e riabilitare registrazione accesso utenti è possibile farlo con le seguenti procedure.  
  
#### <a name="to-start-and-enable-the-ual-service-by-using-the-services-console"></a>Per avviare e abilitare il servizio Registrazione accesso utenti tramite la console Servizi  
  
1.  Accedere al server con un account con privilegi di amministratore locale.  
  
2.  In Server Manager posizionare il puntatore su **Strumenti** e quindi fare clic su **Servizi**.  
  
3.  Scorrere verso il basso e selezionare **Servizio registrazione accessi utente**. Fare clic su **Avviare il servizio**.  
  
4.  Fare clic con il pulsante destro del mouse sul nome del servizio e scegliere **Proprietà**. Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico**e quindi fare clic su **OK**.  
  
#### <a name="to-start-and-enable-ual-from-the-command-line"></a>Per avviare e abilitare Registrazione accesso utenti dalla riga di comando  
  
1.  Accedere al server con credenziali di amministratore locale.  
  
2.  Premere il tasto logo Windows + R e quindi digitare **cmd** per aprire una finestra del prompt dei comandi.  
  
    > [!IMPORTANT]  
    > Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
3.  Digitare **net start ualsvc**, quindi premere INVIO.  
  
4.  Digitare **netsh ualsvc set opmode mode=enable**, quindi premere INVIO.  

Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.
  
È inoltre possibile avviare e riabilitare Registrazione accesso utenti tramite i comandi di Windows PowerShell Start-service e Enable-Ual.  
  
```  
Enable-ual  
```  
  
```  
Start-service ualsvc  
```  
  
## <a name="BKMK_Step2"></a>La raccolta dei dati di registrazione accesso utenti  
Oltre ai cmdlet di PowerShell descritti nella sezione precedente, altri 12 cmdlet può essere usato per raccogliere dati di registrazione accesso utenti:  
  
-   **Get-UalOverview**: fornisce informazioni dettagliate correlate a Registrazione accesso utenti e una cronologia dei prodotti e dei ruoli installati.  
  
-   **Get-UalServerUser**: fornisce i dati di accesso utente client per il server locale o di destinazione.  
  
-   **Get-UalServerDevice**: fornisce i dati di accesso ai dispositivi client per il server locale o di destinazione.  
  
-   **Get-UalUserAccess**: fornisce i dati di accesso utente client per ogni ruolo o prodotto installato nel server locale o di destinazione.  
  
-   **Get-UalDeviceAccess**: fornisce i dati di accesso ai dispositivi client per ogni ruolo o prodotto installato nel server locale o di destinazione.  
  
-   **Get-UalDailyUserAccess**: fornisce i dati di accesso utente client per ogni giorno dell'anno.  
  
-   **Get-UalDailyDeviceAccess**: fornisce i dati di accesso ai dispositivi client per ogni giorno dell'anno.  
  
-   **Get-UalDailyAccess**: fornisce i dati di accesso utente e ai dispositivi client per ogni giorno dell'anno.  
  
-   **Get-UalHyperV**: fornisce i dati delle macchine virtuali pertinenti per il server locale o di destinazione.  
  
-   **Get-UalDns**: fornisce i dati specifici del client DNS per il server DNS locale o di destinazione.  
  
-   **Get-UalSystemId**: fornisce i dati specifici di sistema per l'identificazione univoca del server locale o di destinazione.  
  
`Get-UalSystemId` è progettato per restituire un profilo univoco di un server per tutti gli altri dati dal tale server da correlare.  Se un server modificato in qualsiasi uno dei parametri di `Get-UalSystemId` viene creato un nuovo profilo.  `Get-UalOverview` è progettato per fornire all'amministratore un elenco dei ruoli installati e in uso nel server.  
  
> [!NOTE]  
> Funzionalità di base di servizi File e di stampa e di servizi vengono installate per impostazione predefinita. Gli amministratori possono quindi aspettarsi di visualizzare sempre le informazioni su questi servizi come se fossero installati i ruoli completi. Sono inclusi cmdlet di Registrazione accesso utenti separati per Hyper-V e DNS, in considerazione dei dati univoci raccolti da Registrazione accesso utenti per questi ruoli del server.  
  
Un caso di utilizzo tipico per i cmdlet di Registrazione accesso utenti per un amministratore può essere l'esecuzione di una query in Registrazione accesso utenti per recuperare gli accessi client univoci in un intervallo di date. Questa operazione può essere eseguita in svariati modi. Quello che segue è un metodo consigliato per recuperare gli accessi univoci ai dispositivi in un intervallo di tempo.  
  
```  
PS C:\Windows\system32>Gwmi -Namespace "root\AccessLogging" -query "SELECT * FROM MsftUal_DeviceAccess WHERE LastSeen >='1/01/2013' and LastSeen <='3/31/2013'"  
  
```  
  
Verrà restituito un elenco dettagliato di tutti i dispositivi client univoci, per indirizzo IP, che hanno effettuato richieste al server nell'intervallo di date.  
  
Il valore ‘ActivityCount’ per ogni client univoco è limitato a 65.535 al giorno. La chiamata in WMI da PowerShell, inoltre, è necessaria solo se si eseguono query per data.  Tutti gli altri parametri dei cmdlet di Registrazione accesso utenti possono essere utilizzati nelle query di PowerShell nei modi previsti, come nell'esempio seguente:  
  
```  
PS C:\Windows\system32> Get-UalDeviceAccess -IPAddress "10.36.206.112"  
  
ActivityCount    : 1  
FirstSeen        : 6/23/2012 5:06:50 AM  
IPAddress        : 10.36.206.112  
LastSeen         : 6/23/2012 5:06:50 AM  
ProductName      : Windows Server 2012 Datacenter  
RoleGuid         : 10a9226f-50ee-49d8-a393-9a501d47ce04  
RoleName         : File Server  
TenantIdentifier : 00000000-0000-0000-0000-000000000000  
PSComputerName  
  
```  
  
Registrazione accesso utenti mantiene una cronologia fino a due anni. Per consentire a un amministratore di recuperare i dati di Registrazione accesso utenti quando il servizio è in esecuzione, Registrazione accesso utenti crea una copia del file di database attivo, current.mdb, in un file denominato *GUID.mdb* ogni 24 ore per l'utilizzo da parte del provider WMI.  
  
Nel primo giorno dell'anno, Registrazione accesso utenti creerà un nuovo *GUID.mdb*. Il vecchio file *GUID.mdb* viene mantenuto come archivio per l'utilizzo da parte del provider.  Dopo due anni, il file *GUID.mdb* originale verrà sovrascritto.  
  
> [!IMPORTANT]  
> La procedura seguente dovrebbe essere eseguita solo da un utente avanzato e potrebbe essere usata comunemente da uno sviluppatore per testare la strumentazione creata per le API di Registrazione accesso utente.  
  
#### <a name="to-adjust-the-default-24-hour-interval-to-make-data-visible-to-the-wmi-provider"></a>Per modificare l'intervallo predefinito di 24 ore per rendere visibili i dati al provider WMI  
  
1.  Accedere al server con un account con privilegi di amministratore locale.  
  
2.  Premere il tasto logo Windows + R e quindi digitare **cmd** per aprire una finestra del prompt dei comandi.  
  
3.  Aggiungere il valore del Registro di sistema:  **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\WMI\AutoLogger\Sum\PollingInterval (REG_DWORD)** .  
  
    > [!WARNING]  
    > La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, è consigliabile eseguire il backup dei dati importanti nel computer.  
  
    Nell'esempio seguente viene illustrato come aggiungere un intervallo di due minuti (non consigliato come stato di esecuzione di lungo termine): **REG ADD HKLM\System\CurrentControlSet\Control\WMI\\AutoLogger\Sum /v PollingInterval /t REG\_DWORD /d 120000 /F**  
  
    I valori di tempo sono espressi in millisecondi. Il valore minimo è 60 secondi, il massimo è sette giorni e il predefinito è 24 ore.  
  
4.  Usare la console Servizi per arrestare e riavviare il Servizio registrazione accessi utente.  
  
## <a name="deleting-data-logged-by-ual"></a>Eliminazione dei dati registrati da Registrazione accesso utenti  
Registrazione accesso utenti non è progettato come componente cruciale. È pensato per un impatto minimo sulle operazioni di sistema locali, pur mantenendo un alto livello di affidabilità. Ciò consente anche all'amministratore di eliminare manualmente i database di registrazione accesso utenti e i file (tutti i file nella directory \Windows\System32\LogFilesSUM\) di supporto per soddisfare le esigenze operative.  
  
#### <a name="to-delete-data-logged-by-ual"></a>Per eliminare i dati registrati da Registrazione accesso utenti  
  
1. Arrestare il Servizio registrazione accessi utente.  
  
2. Aprire Esplora risorse.  
  
3. Passare a **\Windows\System32\Logfiles\SUM\\** .  
  
4. Eliminare tutti i file nella cartella.  
  
## <a name="managing-ual-in-high-volume-environments"></a>Gestione di Registrazione accesso utenti in ambienti con volumi elevati  
In questa sezione viene descritto cosa può aspettarsi un amministratore quando si usa Registrazione accesso utenti in un server con un volume elevato di client:  
  
Il numero massimo di accessi che possono essere registrati con Registrazione accesso utenti è 65.535 al giorno.  L'utilizzo di Registrazione accesso utenti non è consigliato nei server connessi direttamente a Internet, come i server Web connessi direttamente a Internet, o in scenari in cui la funzione principale del server è offrire prestazioni estremamente elevate, come in ambienti con carichi di lavoro HPC. Registrazione accesso utenti è destinato principalmente per piccole, medie e gli scenari intranet aziendale in cui sono previsti volumi elevati, ma non così elevati come molte distribuzione che gestiscono il volume di traffico a Internet su base regolare.  
  
**Registrazione accesso utenti in memoria**: dal momento che Registrazione accesso utenti usa Extensible Storage Engine (ESE), i requisiti di memoria per Registrazione accesso utenti sono destinati a crescere nel tempo o in base alla quantità di richieste client. La memoria verrà comunque rilasciata non appena il sistema la richiede per ridurre al minimo l'impatto sulle prestazioni del sistema.  
  
**Registrazione accesso utenti su disco**: i requisiti a livello di disco rigido per Registrazione accesso utenti sono approssimativamente i seguenti:  
  
-   0 record client univoci: 22 MB  
  
-   50.000 record client univoci: 80 MB  
  
-   500.000 record client univoci: 384 MB  
  
-   1.000.000 record client univoci: 729 MB  
  
## <a name="recovering-from-a-corrupt-state"></a>Ripristino da uno stato di danneggiamento  
In questa sezione viene offerta una descrizione di alto livello dell'utilizzo di Extensible Storage Engine (ESE) per Registrazione accesso utenti e vengono indicate le misure che può adottare un amministratore nel caso i dati di Registrazione accesso utenti risultino danneggiati o irrecuperabili.  
  
Registrazione accesso utenti usa ESE per ottimizzare l'utilizzo delle risorse di sistema e per la resistenza di ESE al danneggiamento.  Per altre informazioni sui vantaggi di ESE, vedere l'articolo relativo a [Extensible Storage Engine](https://msdn.microsoft.com/library/windows/desktop/gg269259(v=exchg.10).aspx) su MSDN.  
  
A ogni avvio del servizio Registrazione accesso utenti, ESE esegue un recupero rapido. Per altre informazioni, vedere l'articolo relativo ai [file di Extensible Storage Engine](https://msdn.microsoft.com/library/windows/desktop/gg294069(v=exchg.10).aspx) su MSDN.  
  
In caso di problemi con il ripristino rapido, ESE eseguirà un ripristino a seguito dell'arresto anomalo del sistema. Per altre informazioni, vedere l'articolo relativo alla [funzione JetInit](https://msdn.microsoft.com/library/windows/desktop/gg294068(v=exchg.10).aspx) su MSDN.  
  
Se non è ancora possibile avviare Registrazione accesso utenti con il set esistente di file ESE, verranno eliminati tutti i file nella directory \Windows\System32\LogFiles\SUM\. Dopo l'eliminazione dei file, il servizio Registrazione accessi utente verrà riavviato e verranno creati nuovi file. Il servizio Registrazione accesso utenti riprenderà quindi, come in un computer appena installato.  
  
> [!IMPORTANT]  
> I file nella directory del database di Registrazione accesso utenti non dovrebbero mai essere spostati o modificati. In caso di spostamento o modifica verrà avviata la procedura di ripristino, inclusa la routine di pulizia descritta in questa sezione. Se sono necessari backup della directory \Windows\System32\LogFiles\SUM\, per garantire che un'operazione di ripristino venga eseguita come previsto, eseguire il backup di tutti i file presenti in questa directory.  
  
## <a name="enable-work-folders-usage-license-tracking"></a>Abilitare la verifica delle licenze per l'utilizzo di Cartelle di lavoro  
Il server di Cartelle di lavoro può usare Registrazione accesso utenti per ottenere report sull'utilizzo dei client. A differenza di Registrazione accesso utenti, la registrazione di Cartelle di lavoro non è attivata per impostazione predefinita. Per abilitarla, modificare la chiave del Registro di sistema seguente:  
  
```  
Reg add HKLM\Software\Microsoft\Windows\CurrentVersion\SyncShareSrv /v EnableWorkFoldersUAL /t REG_DWORD /d 1  
```  
  
Dopo aver aggiunto la chiave del Registro di sistema, per abilitare la registrazione è necessario riavviare il servizio SyncShareSvc nel server.  
  
Dopo l'abilitazione della registrazione, ogni volta che un client si connette al server nel canale Registri di Windows\Applicazione vengono registrati due eventi informativi. Per Cartelle di lavoro ogni utente può avere uno o più dispositivi client che si connettono al server e controllano la disponibilità di dati aggiornati ogni 10 minuti. Se il server viene usato da 1000 utenti, ognuno con due dispositivi, i registri applicazioni vengono sovrascritti ogni 70 minuti, di conseguenza la risoluzione di problemi non correlati potrebbe risultare complicata. Per evitare questo problema, è possibile disabilitare temporaneamente il servizio registrazione accesso utenti o aumentare le dimensioni del canale di Windows di windows\applicazione del server.  
  
## <a name="BKMK_Links"></a>Vedere anche  

- [Introduzione a utente registrazione accesso](get-started-with-user-access-logging.md)
  

