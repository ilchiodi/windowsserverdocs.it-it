---
title: Gestire Registrazione accesso utenti
description: Viene descritto come gestire la registrazione accesso utenti
ms.prod: windows-server
ms.technology: manage-user-access-logging
ms.topic: article
ms.assetid: 4f039017-4152-47eb-838e-bb6ef730b638
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ff33102a2197cc961a44290c5b7e4e3e40b0191
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851384"
---
# <a name="manage-user-access-logging"></a>Gestire Registrazione accesso utenti

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo documento illustra come gestire Registrazione accesso utenti.  
  
Registrazione accesso utenti è una funzionalità che gli amministratori del server possono usare per quantificare il numero di richieste client univoche di ruoli e servizi in un server locale.  
  
La funzionalità Registrazione accesso utenti viene installata e abilitata per impostazione predefinita e raccoglie i dati in tempo quasi reale. Per Registrazione accesso utenti è disponibile un numero limitato di opzioni di configurazione. In questo documento tali opzioni vengono descritte insieme agli scopi previsti.  
  
Per altre informazioni sui vantaggi di registrazione accesso utenti, vedere la pagina relativa all' [Introduzione a registrazione accesso utenti](get-started-with-user-access-logging.md).  
  
**Contenuto del documento**  
  
Le opzioni di configurazione descritte in questo documento includono:  
  
-   Disabilitazione e abilitazione del servizio Registrazione accesso utenti  
  
-   Raccolta e rimozione dei dati  
  
-   Eliminazione dei dati registrati da Registrazione accesso utenti  
  
-   Gestione di Registrazione accesso utenti in ambienti con volumi elevati  
  
-   Ripristino da uno stato di danneggiamento  
  
-   Abilitare la verifica delle licenze per l'utilizzo di Cartelle di lavoro   
  
## <a name="disabling-and-enabling-the-ual-service"></a><a name="BKMK_Step1"></a>Disabilitazione e abilitazione del servizio registrazione accesso utenti  
REGISTRAZIONE accesso utenti è abilitato e viene eseguito per impostazione predefinita quando si installa e si avvia per la prima volta un computer che esegue Windows Server 2012 o versione successiva. Gli amministratori potrebbero avere la necessità di disattivare e disabilitare Registrazione accesso utenti per rispettare requisiti di privacy specifici o soddisfare altre esigenze operative. È possibile disattivare registrazione accesso utenti tramite la console servizi, dalla riga di comando o tramite i cmdlet di PowerShell. Tuttavia, per assicurarsi che registrazione accesso utenti non venga eseguito di nuovo al successivo avvio del computer, è necessario disabilitare anche il servizio. Nelle procedure seguenti viene descritto come disattivare e disabilitare registrazione accesso utenti.  
  
> [!NOTE]  
> È possibile usare il cmdlet di PowerShell `Get-Service UALSVC` per recuperare informazioni sul servizio Registrazione accesso utenti e determinare tra l'altro se il servizio è in esecuzione o arrestato e se è abilitato o disabilitato.  
  
#### <a name="to-stop-and-disable-the-ual-service-by-using-the-services-console"></a>Per arrestare e disabilitare il servizio Registrazione accesso utenti tramite la console Servizi  
  
1.  Accedere al server con un account con privilegi di amministratore locale.  
  
2.  In Server Manager posizionare il puntatore su **Strumenti** e quindi fare clic su **Servizi**.  
  
3.  Scorrere verso il basso e selezionare **Servizio registrazione accessi utente**. Fare clic su **Arrestare il servizio**.  
  
4.  A destra\-fare clic sul nome del servizio e selezionare **Proprietà**. Nella scheda **Generale** impostare **Tipo di avvio** su **Disabilitato** e quindi fare clic su **OK**.  
  
#### <a name="to-stop-and-disable-ual-from-the-command-line"></a>Per arrestare e disabilitare Registrazione accesso utenti dalla riga di comando  
  
1.  Accedere al server con un account con privilegi di amministratore locale.  
  
2.  Premere il tasto logo Windows + R e quindi digitare **cmd** per aprire una finestra del prompt dei comandi.  
  
    > [!IMPORTANT]  
    > Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
3.  Digitare **net stop ualsvc**, quindi premere INVIO.  
  
4.  Digitare **netsh ualsvc set opmode mode=disable**, quindi premere INVIO.  
   
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  

È inoltre possibile arrestare e disabilitare Registrazione accesso utenti tramite i comandi di Windows PowerShell Stop-service e Disable-Ual.  
  
```  
Stop-service ualsvc  
```  
  
```  
Disable-ual  
```  
  
Se in un secondo momento si desidera riavviare e riabilitare registrazione accesso utenti, è possibile utilizzare le procedure seguenti.  
  
#### <a name="to-start-and-enable-the-ual-service-by-using-the-services-console"></a>Per avviare e abilitare il servizio Registrazione accesso utenti tramite la console Servizi  
  
1.  Accedere al server con un account con privilegi di amministratore locale.  
  
2.  In Server Manager posizionare il puntatore su **Strumenti** e quindi fare clic su **Servizi**.  
  
3.  Scorrere verso il basso e selezionare **Servizio registrazione accessi utente**. Fare clic su **Avviare il servizio**.  
  
4.  Fare clic con il pulsante destro del mouse sul nome del servizio e scegliere **Proprietà**. Nella scheda **Generale** impostare **Tipo di avvio** su **Automatico** e quindi fare clic su **OK**.  
  
#### <a name="to-start-and-enable-ual-from-the-command-line"></a>Per avviare e abilitare Registrazione accesso utenti dalla riga di comando  
  
1.  Accedere al server con credenziali di amministratore locale.  
  
2.  Premere il tasto logo Windows + R e quindi digitare **cmd** per aprire una finestra del prompt dei comandi.  
  
    > [!IMPORTANT]  
    > Se viene visualizzata la finestra di dialogo **Controllo dell'account utente**, verificare che l'azione indicata sia quella che si desidera eseguire e quindi fare clic su **Sì**.  
  
3.  Digitare **net start ualsvc**, quindi premere INVIO.  
  
4.  Digitare **netsh ualsvc set opmode mode=enable**, quindi premere INVIO.  

Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.
  
È inoltre possibile avviare e riabilitare Registrazione accesso utenti tramite i comandi di Windows PowerShell Start-service e Enable-Ual.  
  
```  
Enable-ual  
```  
  
```  
Start-service ualsvc  
```  
  
## <a name="collecting-ual-data"></a><a name="BKMK_Step2"></a>Raccolta dei dati di registrazione accesso utenti  
Oltre ai cmdlet di PowerShell descritti nella sezione precedente, è possibile usare 12 cmdlet aggiuntivi per raccogliere i dati di registrazione accesso utenti:  
  
-   **Get-UalOverview**: offre informazioni dettagliate correlate a Registrazione accesso utenti e una cronologia dei prodotti e dei ruoli installati.  
  
-   **Get-UalServerUser**: restituisce i dati di accesso utente client per il server locale o di destinazione.  
  
-   **Get-UalServerDevice**: restituisce i dati di accesso ai dispositivi client per il server locale o di destinazione.  
  
-   **Get-UalUserAccess**: fornisce i dati di accesso utente client per ogni ruolo o prodotto installato nel server locale o di destinazione.  
  
-   **Get-UalDeviceAccess**: restituisce i dati di accesso ai dispositivi client per ogni ruolo o prodotto installato nel server locale o di destinazione.  
  
-   **Get-UalDailyUserAccess**: restituisce i dati di accesso utente client per ogni giorno dell'anno.  
  
-   **Get-UalDailyDeviceAccess**: fornisce i dati di accesso ai dispositivi client per ogni giorno dell'anno.  
  
-   **Get-UalDailyAccess**: restituisce i dati di accesso utente e ai dispositivi client per ogni giorno dell'anno.  
  
-   **Get-UalHyperV**: restituisce dati delle macchine virtuali rilevanti per il server locale o di destinazione.  
  
-   **Get-UalDns**: restituisce dati specifici del client DNS del server DNS locale o di destinazione.  
  
-   **Get-UalSystemId**: fornisce i dati specifici di sistema per l'identificazione univoca del server locale o di destinazione.  
  
`Get-UalSystemId` è progettato per restituire un profilo univoco di un server per tutti gli altri dati dal tale server da correlare.  Se un server subisce modifiche in uno dei parametri di `Get-UalSystemId` viene creato un nuovo profilo.  `Get-UalOverview` è progettato per fornire all'amministratore un elenco dei ruoli installati e in uso nel server.  
  
> [!NOTE]  
> Per impostazione predefinita, vengono installate le funzionalità di base dei servizi file e Servizi di stampa e digitalizzazione. Gli amministratori possono quindi aspettarsi di visualizzare sempre le informazioni su questi servizi come se fossero installati i ruoli completi. Sono inclusi cmdlet di registrazione accesso utenti separati per Hyper-V e DNS a causa dei dati univoci raccolti da registrazione accesso utenti per questi ruoli del server.  
  
Un caso di utilizzo tipico per i cmdlet di Registrazione accesso utenti per un amministratore può essere l'esecuzione di una query in Registrazione accesso utenti per recuperare gli accessi client univoci in un intervallo di date. Questa operazione può essere eseguita in diversi modi. Di seguito è riportato un metodo consigliato per eseguire query sugli accessi al dispositivo univoci in un intervallo di date.  
  
```  
PS C:\Windows\system32>Gwmi -Namespace "root\AccessLogging" -query "SELECT * FROM MsftUal_DeviceAccess WHERE LastSeen >='1/01/2013' and LastSeen <='3/31/2013'"  
  
```  
  
Verrà restituito un elenco dettagliato di tutti i dispositivi client univoci, per indirizzo IP, che hanno effettuato richieste al server nell'intervallo di date.  
  
' ActivityCount ' per ogni client univoco è limitato a 65.535 al giorno. Inoltre, la chiamata a WMI da PowerShell è necessaria solo quando si esegue una query in base alla data.  Tutti gli altri parametri dei cmdlet di registrazione accesso utenti possono essere usati all'interno delle query di PowerShell come previsto, come nell'esempio seguente:  
  
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
  
REGISTRAZIONE accesso utenti mantiene la cronologia di un massimo di due anni. Per consentire il recupero dei dati registrazione accesso utenti da un amministratore quando il servizio è in esecuzione, registrazione accesso utenti crea una copia del file di database attivo, Current. mdb, in un file denominato *GUID. mdb* ogni 24 ore per l'utilizzo da parte del provider WMI.  
  
Nel primo giorno dell'anno, Registrazione accesso utenti creerà un nuovo *GUID.mdb*. Il vecchio *GUID. mdb* viene mantenuto come archivio per l'utilizzo da parte del provider.  Dopo due anni, il file *GUID.mdb* originale verrà sovrascritto.  
  
> [!IMPORTANT]  
> La procedura seguente dovrebbe essere eseguita solo da un utente avanzato e potrebbe essere usata comunemente da uno sviluppatore per testare la strumentazione creata per le API di Registrazione accesso utente.  
  
#### <a name="to-adjust-the-default-24-hour-interval-to-make-data-visible-to-the-wmi-provider"></a>Per modificare l'intervallo predefinito di 24 ore per rendere visibili i dati al provider WMI  
  
1.  Accedere al server con un account con privilegi di amministratore locale.  
  
2.  Premere il tasto logo Windows + R e quindi digitare **cmd** per aprire una finestra del prompt dei comandi.  
  
3.  Aggiungere il valore del Registro di sistema:  **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\WMI\AutoLogger\Sum\PollingInterval (REG_DWORD)** .  
  
    > [!WARNING]  
    > È possibile che eventuali modifiche non corrette del Registro di sistema danneggino gravemente il sistema. Prima di apportare modifiche al Registro di sistema, è consigliabile eseguire il backup dei dati importanti nel computer.  
  
    Nell'esempio seguente viene illustrato come aggiungere un intervallo di due minuti (non consigliato come stato di esecuzione a lungo termine): **reg add HKLM\System\CurrentControlSet\Control\WMI\\AutoLogger\Sum/V PollingInterval/T REG\_DWORD/d 120000/f**  
  
    I valori di tempo sono espressi in millisecondi. Il valore minimo è 60 secondi, il massimo è sette giorni e il predefinito è 24 ore.  
  
4.  Usare la console Servizi per arrestare e riavviare il Servizio registrazione accessi utente.  
  
## <a name="deleting-data-logged-by-ual"></a>Eliminazione dei dati registrati da Registrazione accesso utenti  
Registrazione accesso utenti non è progettato come componente cruciale. È pensato per un impatto minimo sulle operazioni di sistema locali, pur mantenendo un alto livello di affidabilità. Ciò consente anche all'amministratore di eliminare manualmente il database registrazione accesso utenti e i file di supporto (tutti i file nella directory \Windows\System32\LogFilesSUM\) per soddisfare le esigenze operative.  
  
#### <a name="to-delete-data-logged-by-ual"></a>Per eliminare i dati registrati da Registrazione accesso utenti  
  
1. Arrestare il Servizio registrazione accessi utente.  
  
2. Aprire Esplora risorse.  
  
3. Passare a **\Windows\System32\Logfiles\SUM\\** .  
  
4. Eliminare tutti i file nella cartella.  
  
## <a name="managing-ual-in-high-volume-environments"></a>Gestione di Registrazione accesso utenti in ambienti con volumi elevati  
In questa sezione viene descritto cosa può aspettarsi un amministratore quando si usa Registrazione accesso utenti in un server con un volume elevato di client:  
  
Il numero massimo di accessi che possono essere registrati con Registrazione accesso utenti è 65.535 al giorno.  REGISTRAZIONE accesso utenti non è consigliato per l'uso nei server connessi direttamente a Internet, ad esempio i server Web connessi direttamente a Internet, o in scenari in cui le prestazioni estremamente elevate sono la funzione principale del server (ad esempio negli ambienti di carico di lavoro HPC). REGISTRAZIONE accesso utenti è destinato principalmente a scenari Intranet piccoli, medi e aziendali in cui è previsto un volume elevato, ma non il numero massimo di distribuzioni che gestiscono il volume di traffico con connessione Internet a intervalli regolari.  
  
**Registrazione accesso utenti in memoria**: poiché registrazione accesso utenti utilizza Extensible Storage Engine (ESE), i requisiti di memoria di registrazione accesso utenti aumenteranno nel tempo o in base alla quantità di richieste client. La memoria verrà comunque rilasciata non appena il sistema la richiede per ridurre al minimo l'impatto sulle prestazioni del sistema.  
  
**Registrazione accesso utenti su disco**: i requisiti del disco rigido di registrazione accesso utenti sono approssimativamente quelli indicati di seguito:  
  
-   0 record client univoci: 22 MB  
  
-   50.000 record client univoci: 80 MB  
  
-   500.000 record client univoci: 384 MB  
  
-   1\.000.000 di record client univoci: 729 MB  
  
## <a name="recovering-from-a-corrupt-state"></a>Ripristino da uno stato di danneggiamento  
Questa sezione illustra l'uso di Extensible Storage Engine (ESE) di registrazione accesso utenti a un livello elevato e ciò che un amministratore può eseguire se i dati di registrazione accesso utenti sono danneggiati o irrecuperabili.  
  
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
  
Dopo l'abilitazione della registrazione, ogni volta che un client si connette al server nel canale Registri di Windows\Applicazione vengono registrati due eventi informativi. Per Cartelle di lavoro ogni utente può avere uno o più dispositivi client che si connettono al server e controllano la disponibilità di dati aggiornati ogni 10 minuti. Se il server viene usato da 1000 utenti, ognuno con due dispositivi, i registri applicazioni vengono sovrascritti ogni 70 minuti, di conseguenza la risoluzione di problemi non correlati potrebbe risultare complicata. Per evitare questo problema, è possibile disabilitare temporaneamente il servizio registrazione accessi utente o aumentare le dimensioni del canale registri Windows\Applicazione di Windows del server.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Vedere anche  

- [Introduzione alla registrazione accesso utenti](get-started-with-user-access-logging.md)
  

