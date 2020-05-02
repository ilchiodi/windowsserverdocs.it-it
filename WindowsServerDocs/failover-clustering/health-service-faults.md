---
title: Errori Servizio integrità
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 5fe2f98c89d97325c1f59dc6ba292831e0ffa5ff
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720557"
---
# <a name="health-service-faults"></a>Errori Servizio integrità

> Si applica a: Windows Server 2019, Windows Server 2016

## <a name="what-are-faults"></a>Che cosa sono gli errori

Il Servizio integrità monitora costantemente il cluster Spazi di archiviazione diretta per rilevare i problemi e generare "errori". Un nuovo cmdlet Visualizza tutti gli errori correnti, consentendo di verificare facilmente l'integrità della distribuzione senza esaminare ogni entità o funzionalità a turno. Gli errori sono progettati per essere precisi, facili da comprendere e correggibili.  

Ogni errore contiene cinque campi importanti:  

-   Gravità
-   Descrizione del problema
-   Passaggi successivi consigliati per risolvere il problema
-   Informazioni di identificazione per l'entità che ha generato l’errore
-   La posizione fisica (se applicabile)

Ad esempio, di seguito viene riportato un errore tipico:  

```
Severity: MINOR                                         
Reason: Connectivity has been lost to the physical disk.                           
Recommendation: Check that the physical disk is working and properly connected.    
Part: Manufacturer Contoso, Model XYZ9000, Serial 123456789                        
Location: Seattle DC, Rack B07, Node 4, Slot 11
```

 >[!NOTE]
 > La posizione fisica viene derivata dalla configurazione del dominio di errore. Per ulteriori informazioni sui domini di errore, vedere [domini di errore in Windows Server 2016](fault-domains.md). Se non si specifica questa informazione, il campo della posizione sarà meno utile, ad esempio potrebbe visualizzare solo il numero di slot.  

## <a name="root-cause-analysis"></a>Analisi della causa radice

Il Servizio integrità è in grado di valutare la causa potenziale tra le entità di errore per identificare e combinare errori, che sono conseguenze dello stesso problema sottostante. Riconoscendo gli effetti concatenati, è possibile generare report più concisi. Se, ad esempio, un server è inattivo, è previsto che anche le unità all'interno del server non siano in funzione di connettività. Viene pertanto generato un solo errore per la causa radice, in questo caso il server.  

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Per visualizzare eventuali errori correnti in PowerShell, eseguire questo cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

In questo modo vengono restituiti tutti gli errori che influiscono sul cluster Spazi di archiviazione diretta globale. Spesso questi errori si riferiscono all'hardware o alla configurazione. Se non sono presenti errori, questo cmdlet non restituirà alcun risultato.  

>[!NOTE]
> In un ambiente non di produzione e, a proprio rischio, è possibile sperimentare questa funzionalità attivando gli errori, ad esempio rimuovendo un disco fisico o arrestando un nodo. Una volta visualizzato l'errore, inserire nuovamente il disco fisico o riavviare il nodo e l'errore scomparirà nuovamente.

È anche possibile visualizzare gli errori che interessano solo volumi o condivisioni file specifici con i cmdlet seguenti:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Vengono restituiti tutti gli errori che interessano solo la condivisione file o il volume specifico. Spesso questi errori riguardano la pianificazione della capacità, la resilienza dei dati o funzionalità come la qualità del servizio di archiviazione o la replica di archiviazione. 

## <a name="usage-in-net-and-c"></a>Utilizzo in .NET e C #

### <a name="connect"></a>Connessione

Per eseguire una query sulla Servizio integrità, è necessario stabilire una **CimSession** con il cluster. A tale scopo, sono necessari alcuni elementi disponibili solo in .NET completo, ovvero non è possibile eseguire facilmente questa operazione direttamente da un'app Web o per dispositivi mobili. Questi esempi di codice utilizzeranno\#C, la scelta più semplice per questo livello di accesso ai dati.

```
using System.Security;
using Microsoft.Management.Infrastructure;

public CimSession Connect(string Domain = "...", string Computer = "...", string Username = "...", string Password = "...")
{
    SecureString PasswordSecureString = new SecureString();
    foreach (char c in Password)
    {
        PasswordSecureString.AppendChar(c);
    }

    CimCredential Credentials = new CimCredential(
        PasswordAuthenticationMechanism.Default, Domain, Username, PasswordSecureString);
    WSManSessionOptions SessionOptions = new WSManSessionOptions();
    SessionOptions.AddDestinationCredentials(Credentials);
    Session = CimSession.Create(Computer, SessionOptions);
    return Session;
}
```

Il nome utente specificato deve essere un amministratore locale del computer di destinazione.

È consigliabile costruire la password **SecureString** direttamente dall'input dell'utente in tempo reale, quindi la password non viene mai archiviata in memoria in testo non crittografato. Questo consente di attenuare una serie di problemi di sicurezza. In pratica, tuttavia, la creazione di questo approccio è comune a scopo di creazione di prototipi.

### <a name="discover-objects"></a>Individuazione oggetti

Con la **CimSession** stabilita, è possibile eseguire una query Strumentazione gestione Windows (WMI) nel cluster.

Prima di poter ottenere errori o metriche, è necessario ottenere istanze di diversi oggetti rilevanti. In primo luogo **,\_MSFT StorageSubSystem** che rappresenta spazi di archiviazione diretta nel cluster. Usando questo, è possibile ottenere ogni **StorageNode\_MSFT** nel cluster e ogni **volume MSFT\_**, ovvero i volumi di dati. Infine, sarà necessario anche il **StorageHealth\_MSFT**, il servizio integrità stesso.

```
CimInstance Cluster;
List<CimInstance> Nodes;
List<CimInstance> Volumes;
CimInstance HealthService;

public void DiscoverObjects(CimSession Session)
{
    // Get MSFT_StorageSubSystem for Storage Spaces Direct
    Cluster = Session.QueryInstances(@"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageSubSystem")
        .First(Instance => (Instance.CimInstanceProperties["FriendlyName"].Value.ToString()).Contains("Cluster"));

    // Get MSFT_StorageNode for each cluster node
    Nodes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageNode", null, "StorageSubSystem", "StorageNode").ToList();

    // Get MSFT_Volumes for each data volume
    Volumes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToVolume", null, "StorageSubSystem", "Volume").ToList();

    // Get MSFT_StorageHealth itself
    HealthService = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageHealth", null, "StorageSubSystem", "StorageHealth").First();
}
```

Si tratta degli stessi oggetti che si ottengono in PowerShell usando cmdlet come **Get-StorageSubSystem**, **Get-StorageNode**e **Get-volume**.

È possibile accedere a tutte le stesse proprietà, documentate in [classi API di gestione dell'archiviazione](https://msdn.microsoft.com/library/windows/desktop/hh830612(v=vs.85).aspx).

```
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

### <a name="query-faults"></a>Errori di query

Richiama la **diagnostica** per ottenere gli errori correnti con ambito **CimInstance**di destinazione, ovvero il cluster o qualsiasi volume.

L'elenco completo degli errori disponibili in ogni ambito di Windows Server 2016 è illustrato di seguito.

```       
public void GetFaults(CimSession Session, CimInstance Target)
{
    // Set Parameters (None)
    CimMethodParametersCollection FaultsParams = new CimMethodParametersCollection();
    // Invoke API
    CimMethodResult Result = Session.InvokeMethod(Target, "Diagnose", FaultsParams);
    IEnumerable<CimInstance> DiagnoseResults = (IEnumerable<CimInstance>)Result.OutParameters["DiagnoseResults"].Value;
    // Unpack
    if (DiagnoseResults != null)
    {
        foreach (CimInstance DiagnoseResult in DiagnoseResults)
        {
            // TODO: Whatever you want!
        }
    }
}
```

### <a name="optional-myfault-class"></a>Facoltativo: classe di errore

Potrebbe essere utile creare e rendere permanente la rappresentazione degli errori. Questa classe di **errore** , ad esempio, archivia diverse proprietà chiave degli errori, tra cui **FaultId**, che può essere usato in un secondo momento per associare le notifiche di aggiornamento o rimozione oppure per deduplicare nel caso in cui lo stesso errore venga rilevato più volte, per qualsiasi motivo.

```       
public class MyFault {
    public String FaultId { get; set; }
    public String Reason { get; set; }
    public String Severity { get; set; }
    public String Description { get; set; }
    public String Location { get; set; }

    // Constructor
    public MyFault(CimInstance DiagnoseResult)
    {
        CimKeyedCollection<CimProperty> Properties = DiagnoseResult.CimInstanceProperties;
        FaultId     = Properties["FaultId"                  ].Value.ToString();
        Reason      = Properties["Reason"                   ].Value.ToString();
        Severity    = Properties["PerceivedSeverity"        ].Value.ToString();
        Description = Properties["FaultingObjectDescription"].Value.ToString();
        Location    = Properties["FaultingObjectLocation"   ].Value.ToString();
    }
}
```

```
List<MyFault> Faults = new List<MyFault>;

foreach (CimInstance DiagnoseResult in DiagnoseResults)
{
    Faults.Add(new Fault(DiagnoseResult));
}
```

L'elenco completo delle proprietà in ogni errore (**DiagnoseResult**) è illustrato di seguito.

### <a name="fault-events"></a>Eventi di errore

Quando vengono creati, rimossi o aggiornati errori, il Servizio integrità genera eventi WMI. Questi elementi sono essenziali per mantenere sincronizzato lo stato dell'applicazione senza frequenti polling e possono essere utili per determinare quando inviare avvisi di posta elettronica, ad esempio. Per sottoscrivere questi eventi, questo codice di esempio USA di nuovo il modello di progettazione Observer.

Per prima cosa, sottoscrivere gli eventi **StorageFaultEvent di MSFT\_** .

```      
public void ListenForFaultEvents()
{
    IObservable<CimSubscriptionResult> Events = Session.SubscribeAsync(
        @"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageFaultEvent");
    // Subscribe the Observer
    FaultsObserver<CimSubscriptionResult> Observer = new FaultsObserver<CimSubscriptionResult>(this);
    IDisposable Disposeable = Events.Subscribe(Observer);
}   
```

Implementare quindi un Observer il cui metodo **OnNext ()** verrà richiamato ogni volta che viene generato un nuovo evento.

Ogni evento contiene un oggetto **ChangeType** che indica se viene creato, rimosso o aggiornato un errore e il **FaultId**pertinente.

Contengono inoltre tutte le proprietà dell'errore stesso.

```
class FaultsObserver : IObserver
{
    public void OnNext(T Event)
    {
        // Cast
        CimSubscriptionResult SubscriptionResult = Event as CimSubscriptionResult;

        if (SubscriptionResult != null)
        {
            // Unpack            
            CimKeyedCollection<CimProperty> Properties = SubscriptionResult.Instance.CimInstanceProperties;
            String ChangeType = Properties["ChangeType"].Value.ToString();
            String FaultId = Properties["FaultId"].Value.ToString();

            // Create
            if (ChangeType == "0")
            {
                Fault MyNewFault = new MyFault(SubscriptionResult.Instance);
                // TODO: Whatever you want!
            }
            // Remove
            if (ChangeType == "1")
            {
                // TODO: Use FaultId to find and delete whatever representation you have...
            }
            // Update
            if (ChangeType == "2")
            {
                // TODO: Use FaultId to find and modify whatever representation you have...
            }
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Nothing
    }
}
```

### <a name="understand-fault-lifecycle"></a>Informazioni sul ciclo di vita degli errori

Gli errori non devono essere contrassegnati come "visualizzati" o risolti dall'utente. Vengono creati quando il Servizio integrità osserva un problema e vengono rimossi automaticamente e solo quando il Servizio integrità non è più in grado di osservare il problema. In generale, questo indica che il problema è stato risolto.

Tuttavia, in alcuni casi, gli errori possono essere rilevati dal Servizio integrità, ad esempio dopo il failover o a causa della connettività intermittente e così via. Per questo motivo, può essere utile rendere permanente la rappresentazione degli errori, pertanto è possibile deduplicare facilmente. Questa operazione è particolarmente importante se si inviano avvisi di posta elettronica o equivalenti.

### <a name="properties-of-faults"></a>Proprietà degli errori

Questa tabella presenta diverse proprietà chiave dell'oggetto fault. Per lo schema completo, controllare la **classe\_MSFT StorageDiagnoseResult** in *StorageWMI. mof*.

| **Proprietà**              | **Esempio**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| Tipo FaultType                 | Microsoft. Health. tipo FaultType. volume. Capacity                      |
| Motivo                    | "Lo spazio disponibile nel volume è esaurito."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, UR 25, slot 11                                        |
| RecommendedActions        | {"Espandere il volume.", "migrare i carichi di lavoro ad altri volumi"}   |

**FaultId** Univoco nell'ambito di un cluster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {"Informational", "Warning" e "Error"} oppure colori equivalenti, ad esempio blu, giallo e rosso.

**FaultingObjectDescription** Informazioni sulla parte per l'hardware, in genere vuoti per gli oggetti software.

**FaultingObjectLocation** Informazioni sulla posizione per l'hardware, in genere vuoti per gli oggetti software.

**RecommendedActions** Elenco di azioni consigliate, indipendenti e senza un ordine particolare. Attualmente, questo elenco è spesso di lunghezza 1.

## <a name="properties-of-fault-events"></a>Proprietà degli eventi di errore

Questa tabella presenta diverse proprietà chiave dell'evento di errore. Per lo schema completo, controllare la **classe\_MSFT StorageFaultEvent** in *StorageWMI. mof*.

Si noti l'oggetto **ChangeType**, che indica se viene creato, rimosso o aggiornato un errore e **FaultId**. Un evento contiene anche tutte le proprietà dell'errore interessato.

| **Proprietà**              | **Esempio**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| Tipo FaultType                 | Microsoft. Health. tipo FaultType. volume. Capacity                      |
| Motivo                    | "Lo spazio disponibile nel volume è esaurito."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, UR 25, slot 11                                        |
| RecommendedActions        | {"Espandere il volume.", "migrare i carichi di lavoro ad altri volumi"}   |

**ChangeType** ChangeType = {0, 1, 2} = {"create", "Remove", "Update"}.

## <a name="coverage"></a>Copertura

In Windows Server 2016, il Servizio integrità fornisce la copertura degli errori seguente:  

### <a name="physicaldisk-8"></a>**PhysicalDisk (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. PhysicalDisk. FailedMedia
* Gravità: Avviso
* Motivo: *"il disco fisico non è riuscito".*
* RecommendedAction: *"sostituire il disco fisico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. PhysicalDisk. LostCommunication
* Gravità: Avviso
* Motivo: *"la connettività è stata persa sul disco fisico".*
* RecommendedAction: *"verificare che il disco fisico sia funzionante e connesso correttamente".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. PhysicalDisk. non risponde
* Gravità: Avviso
* Motivo: *"il disco fisico sta mostrando una ricorrenza di non risposta".*
* RecommendedAction: *"sostituire il disco fisico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. PhysicalDisk. PredictiveFailure
* Gravità: Avviso
* Motivo: *"si è verificato un errore del disco fisico a breve."*
* RecommendedAction: *"sostituire il disco fisico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. PhysicalDisk. UnsupportedHardware
* Gravità: Avviso
* Motivo: *"il disco fisico è in quarantena perché non è supportato dal fornitore della soluzione."*
* RecommendedAction: *"sostituire il disco fisico con hardware supportato".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. PhysicalDisk. UnsupportedFirmware
* Gravità: Avviso
* Motivo: *"il disco fisico è in quarantena perché la versione del firmware non è supportata dal fornitore della soluzione."*
* RecommendedAction: *"aggiornare il firmware del disco fisico alla versione di destinazione".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. PhysicalDisk. UnrecognizedMetadata
* Gravità: Avviso
* Motivo: *"il disco fisico contiene metadati non riconosciuti".*
* RecommendedAction: *"questo disco può contenere dati da un pool di archiviazione sconosciuto. Assicurarsi prima di tutto che non vi siano dati utili su questo disco, quindi reimpostare il disco ".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. PhysicalDisk. FailedFirmwareUpdate
* Gravità: Avviso
* Motivo: *"Impossibile eseguire l'aggiornamento del firmware sul disco fisico".*
* RecommendedAction: *"provare a usare un altro binario del firmware".*

### <a name="virtual-disk-2"></a>**Disco virtuale (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. Virtualdisks con. NeedsRepair
* Gravità: informativo
* Motivo: *"alcuni dati in questo volume non sono completamente resilienti. Rimane accessibile ".*
* RecommendedAction: *"ripristino della resilienza dei dati".*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. Virtualdisks con. Detached
* Gravità: Critica
* Motivo: *"il volume è inaccessibile. Alcuni dati potrebbero andare perduti ".*
* RecommendedAction: *"controllare la connettività fisica e/o di rete di tutti i dispositivi di archiviazione. Potrebbe essere necessario eseguire il ripristino dal backup ".*

### <a name="pool-capacity-1"></a>**Capacità pool (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. StoragePool. InsufficientReserveCapacityFault
* Gravità: Avviso
* Motivo: *"il pool di archiviazione non dispone della capacità minima di riserva consigliata. Questo potrebbe limitare la capacità di ripristinare la resilienza dei dati in caso di errori dell'unità. "*
* RecommendedAction: *"aggiungere capacità aggiuntiva al pool di archiviazione o liberare capacità. La riserva consigliata minima varia a seconda della distribuzione, ma è di circa 2 unità di capacità ".*

### <a name="volume-capacity-2sup1sup"></a>**Capacità volume (2)**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. volume. Capacity
* Gravità: Avviso
* Motivo: *"lo spazio disponibile nel volume è esaurito."*
* RecommendedAction: *"espandere il volume o eseguire la migrazione dei carichi di lavoro ad altri volumi".*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. volume. Capacity
* Gravità: Critica
* Motivo: *"lo spazio disponibile nel volume è esaurito."*
* RecommendedAction: *"espandere il volume o eseguire la migrazione dei carichi di lavoro ad altri volumi".*

### <a name="server-3"></a>**Server (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. Server. Down
* Gravità: Critica
* Motivo: *"Impossibile raggiungere il server."*
* RecommendedAction: *"avvia o Sostituisci server".*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. Server. isolated
* Gravità: Critica
* Motivo: *"il server è isolato dal cluster a causa di problemi di connettività".*
* RecommendedAction: *"se l'isolamento è permanente, controllare le reti o eseguire la migrazione dei carichi di lavoro ad altri nodi".*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. Server. Quarantined
* Gravità: Critica
* Motivo: *"il server è in quarantena dal cluster a causa di errori ricorrenti".*
* RecommendedAction: *"sostituire il server o correggere la rete".*

### <a name="cluster-1"></a>**Cluster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. ClusterQuorumWitness. Error
* Gravità: Critica
* Motivo: *"il cluster si è verificato un errore del server."*
* RecommendedAction: *"controllare la risorsa del server di controllo del mirroring e riavviare se necessario. Avviare o sostituire i server non riusciti ".*

### <a name="network-adapterinterface-4"></a>**Interfaccia/scheda di rete (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. NetworkAdapter. disconnected
* Gravità: Avviso
* Motivo: *"l'interfaccia di rete è stata disconnessa".*
* RecommendedAction: *"riconnettere il cavo di rete".*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. interfaccia. Missing
* Gravità: Avviso
* Motivo: *"nel server {server} mancano le schede di rete connesse alla rete cluster {cluster network}".*
* RecommendedAction: *"connettere il server alla rete cluster mancante".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. NetworkAdapter. hardware
* Gravità: Avviso
* Motivo: *"l'interfaccia di rete ha riscontrato un errore hardware".*
* RecommendedAction: *"sostituire la scheda di interfaccia di rete".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. NetworkAdapter. disabled
* Gravità: Avviso
* Motivo: *"l'interfaccia di rete {Network Interface} non è abilitata e non è in uso".*
* RecommendedAction: *"Abilita l'interfaccia di rete."*

### <a name="enclosure-6"></a>**Enclosure (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. StorageEnclosure. LostCommunication
* Gravità: Avviso
* Motivo: *"la comunicazione è stata persa nell'enclosure di archiviazione".*
* RecommendedAction: *"avvia o Sostituisci l'enclosure di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. StorageEnclosure. FanError
* Gravità: Avviso
* Motivo: *"la ventola alla posizione {position} dell'enclosure di archiviazione non è riuscita."*
* RecommendedAction: *"sostituire la ventola nell'enclosure di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. StorageEnclosure. CurrentSensorError
* Gravità: Avviso
* Motivo: *"il sensore corrente alla posizione {position} dell'enclosure di archiviazione non è riuscito."*
* RecommendedAction: *"sostituire un sensore corrente nell'enclosure di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. StorageEnclosure. VoltageSensorError
* Gravità: Avviso
* Motivo: *"il sensore di tensione alla posizione {position} dell'enclosure di archiviazione non è riuscito".*
* RecommendedAction: *"sostituire un sensore di tensione nell'enclosure di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. StorageEnclosure. IoControllerError
* Gravità: Avviso
* Motivo: *"il controller io nella posizione {position} dell'enclosure di archiviazione non è riuscito".*
* RecommendedAction: *"sostituire un controller di io nello chassis di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. StorageEnclosure. TemperatureSensorError
* Gravità: Avviso
* Motivo: *"il sensore di temperatura alla posizione {position} dell'enclosure di archiviazione non è riuscito."*
* RecommendedAction: *"sostituire un sensore di temperatura nell'enclosure di archiviazione".*

### <a name="firmware-rollout-3"></a>**Implementazione del firmware (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. FaultDomain. FailedMaintenanceMode
* Gravità: Avviso
* Motivo: *"attualmente non è in grado di eseguire lo stato di avanzamento del firmware".*
* RecommendedAction: *"verificare che tutti gli spazi di archiviazione siano integri e che nessun dominio di errore sia attualmente in modalità di manutenzione".*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. FaultDomain. FirmwareVerifyVersionFaile
* Gravità: Avviso
* Motivo: *"la distribuzione del firmware è stata annullata a causa di informazioni illeggibili o impreviste sulla versione del firmware dopo l'applicazione di un aggiornamento del firmware".*
* RecommendedAction: *"riavvio del firmware dopo la risoluzione del problema del firmware".*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. FaultDomain. TooManyFailedUpdates
* Gravità: Avviso
* Motivo: *"implementazione del firmware annullata a causa di un numero eccessivo di dischi fisici che non riescono a eseguire un tentativo di aggiornamento del firmware"*
* RecommendedAction: *"riavvio del firmware dopo la risoluzione del problema del firmware".*

### <a name="storage-qos-3sup2sup"></a>**QoS di archiviazione (3)**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. StorQos. InsufficientThroughput
* Gravità: Avviso
* Motivo: *"la velocità effettiva di archiviazione è insufficiente per soddisfare le riservate".*
* RecommendedAction: *"riconfigurare i criteri QoS di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. StorQos. LostCommunication
* Gravità: Avviso
* Motivo: *"Gestione criteri QoS di archiviazione ha perso la comunicazione con il volume".*
* RecommendedAction: *"riavviare i nodi {Nodes}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>Tipo FaultType: Microsoft. Health. tipo FaultType. StorQos. MisconfiguredFlow
* Gravità: Avviso
* Motivo: *"uno o più consumer di archiviazione (in genere macchine virtuali) usano un criterio non esistente con ID {ID}."*
* RecommendedAction: *"ricreare i criteri QoS di archiviazione mancanti".*

<sup>1</sup> indica che il volume ha raggiunto il 80% completo (gravità secondaria) o il 90% completo (gravità principale).  
<sup>2</sup> indica che alcuni file con estensione vhd nel volume non hanno soddisfatto il numero minimo di IOPS per oltre il 10% (minore), il 30% (principale) o il 50% (critico) della finestra a 24 ore in sequenza.  

>[!NOTE]
> L'integrità dei componenti dell’alloggiamento di archiviazione, ad esempio ventole, alimentatori e sensori deriva da servizi SES (SCSI Enclosure Services). Se il fornitore non specifica queste informazioni, Servizio integrità non può visualizzarlo.  

## <a name="see-also"></a>Vedere anche

- [Servizio integrità in Windows Server 2016](health-service-overview.md)
