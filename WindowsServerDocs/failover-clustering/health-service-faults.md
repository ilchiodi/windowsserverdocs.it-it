---
title: "Errori del servizio integrità"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: c9e1fb4568ee93739c49ccc1a13106b09161c5f3
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-faults"></a>Errori del servizio integrità
> Si applica a Windows Server 2016

## <a name="what-are-faults"></a>Quali sono gli errori?

Il servizio integrità controlla costantemente il cluster di spazi di archiviazione diretta per rilevare i problemi e generare "errori". Un nuovo cmdlet Visualizza tutti gli errori correnti, che consente di verificare l'integrità della distribuzione senza esaminare ogni singola entità o funzionalità a sua volta facilmente. Gli errori sono progettati per essere precisi, facili da capire ed eseguibili.  

Ogni errore contiene cinque campi importanti:  

-   Livello di gravità
-   Descrizione del problema
-   Passaggi successivi consigliati per risolvere il problema
-   Informazioni di identificazione per l'entità che
-   La posizione fisica (se applicabile)

Ad esempio, ecco un errore tipico:  

```
Severity: MINOR                                         
Reason: Connectivity has been lost to the physical disk.                           
Recommendation: Check that the physical disk is working and properly connected.    
Part: Manufacturer Contoso, Model XYZ9000, Serial 123456789                        
Location: Seattle DC, Rack B07, Node 4, Slot 11
```

 >[!NOTE]
 > La posizione fisica viene derivata dalla configurazione del dominio di errore. Per ulteriori informazioni sui domini di errore, vedere [domini di errore in Windows Server 2016](fault-domains.md). Se non si specifica questa informazione, il campo della posizione sarà meno utile, ad esempio, potrebbe visualizzare solo il numero di slot.  

## <a name="root-cause-analysis"></a>Analisi della causa principale

Il servizio integrità può valutare la causalità potenziale tra le entità per identificare e combinare gli errori causati dello stesso problema sottostante l'attivazione. Riconoscendo dell'effetto, in questo modo per generare report più concisi. Ad esempio, se un server è inattivo, è previsto rispetto a qualsiasi unità all'interno del server saranno anche senza la connettività. Di conseguenza, verrà generato un solo errore relativo alla causa principale, in questo caso, il server.  

## <a name="usage-in-powershell"></a>Utilizzo di PowerShell

Per visualizzare tutti gli errori correnti in PowerShell, eseguire questo cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

Restituisce tutti gli errori che interessano il cluster di spazi di archiviazione diretta generale. In genere questi errori riguardano configurazione o hardware. Se non vengono rilevati errori, questo cmdlet restituirà alcun valore.  

>[!NOTE]
> In un ambiente non di produzione e a proprio rischio, è possibile sperimentare questa funzionalità generando errori manualmente, ad esempio, mediante la rimozione di un disco fisico o arresto di un nodo. Una volta è visualizzato l'errore, reinserire il disco fisico oppure riavviare che il nodo e l'errore non verrà più visualizzato nuovamente.

È inoltre possibile visualizzare gli errori che interessano solo volumi specifici o condivisioni file con i cmdlet seguenti:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Restituisce tutti gli errori che interessano solo lo specifico volume o condivisione file. In genere questi errori riguardano la pianificazione della capacità, resilienza dei dati o funzionalità come Storage Quality of Service o la Replica di archiviazione. 

## <a name="usage-in-net-and-c"></a>Utilizzo in .NET e c#

### <a name="connect"></a>Connettersi

Per eseguire la query del servizio integrità, è necessario stabilire un **CimSession** con il cluster. A tale scopo, è necessario alcune funzionalità che sono disponibili solo in completa di .NET, pertanto è possibile facilmente effettuare questa operazione direttamente da un web o app mobile. Questi esempi di codice utilizzerà c \ #, la scelta più semplice per questo livello di accesso ai dati.

``` 
...
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

L'utente specificato deve essere un amministratore locale del Computer di destinazione.

È consigliabile che la Password costruire **SecureString** direttamente dall'input dell'utente in tempo reale, pertanto la password non verrà mai archiviata nella memoria come testo non crittografato. Ciò consente di ridurre un'ampia gamma di problemi di sicurezza. Ma in pratica, è comune per scopi di creazione di prototipi costruirla in come sopra.

### <a name="discover-objects"></a>Individuare oggetti

Con la **CimSession** stabilita, è possibile eseguire query Strumentazione gestione Windows (WMI) nel cluster.

Prima di poter accedere gli errori o metriche, è necessario ottenere istanze di diversi oggetti pertinenti. Prima di tutto, il **MSFT\_StorageSubSystem** che rappresenta nel cluster di spazi di archiviazione diretta. Che utilizza, è possibile ottenere ogni **MSFT\_StorageNode** del cluster e ogni **MSFT\_Volume**, i volumi di dati. Infine, è necessario il **MSFT\_StorageHealth**, il servizio integrità stesso, troppo.

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

Questi sono gli stessi oggetti Ricevi in PowerShell tramite i cmdlet come **Get-StorageSubSystem**, **Get-StorageNode**, e **Get-Volume**.

È possibile accedere le stesse proprietà, documentate all'indirizzo [classi API di gestione archiviazione](https://msdn.microsoft.com/en-us/library/windows/desktop/hh830612(v=vs.85).aspx).

```
...
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

### <a name="query-faults"></a>Errori di query

Richiamare **diagnosi** per ottenere tutti gli errori correnti ambiti alla destinazione **CimInstance**, che essere il cluster o qualsiasi volume.

L'elenco completo degli errori disponibili in ogni ambito in Windows Server 2016 è descritta di seguito.

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

### <a name="optional-myfault-class"></a>Facoltativo: MyFault classe

Quali potrebbe essere utile per la creazione e mantenere i propria rappresentazione di errori. Ad esempio, questo **MyFault** classe archivia diverse proprietà chiave di errori, inclusi il **FaultId**, che potranno essere successivamente utilizzati per associare l'aggiornamento o rimuovere le notifiche o per deduplicare nel caso in cui l'errore stesso è rilevato più volte, per qualsiasi motivo.

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

L'elenco completo delle proprietà in ogni errore (**DiagnoseResult**) indicate di seguito.

### <a name="fault-events"></a>Eventi di errore

Quando gli errori vengono creati, rimossi o aggiornati, il servizio integrità genera eventi WMI. Questi strumenti sono essenziali per mantenere sincronizzati senza frequenti polling dello stato dell'applicazione e può essere utile elementi come stabilire quando è necessario inviare avvisi di posta elettronica, ad esempio. Per sottoscrivere questi eventi, questo codice di esempio Usa il modello di progettazione Observer nuovamente.

Prima di tutto, sottoscrivere **MSFT\_StorageFaultEvent** eventi.

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

Implementa quindi un Observer cui **OnNext()** metodo viene richiamato ogni volta che viene generato un nuovo evento.

Ogni evento contiene **ChangeType** che indica se un errore di creazione, rimossi o aggiornati e il **FaultId**.

Inoltre, contengono tutte le proprietà dell'errore stesso.

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

### <a name="understand-fault-lifecycle"></a>Comprendere l'errore del ciclo di vita

Gli errori non deve essere contrassegnato come "rilevamento" o risolto dall'utente. Vengono create quando il servizio integrità rileva un problema e vengono rimossi automaticamente e solo quando il servizio integrità non è più possibile osservare il problema. In generale, ciò indica che è stato risolto il problema.

Tuttavia, in alcuni casi, gli errori potrebbero rilevarli di nuovo dal servizio integrità (ad esempio, dopo il failover, oppure a causa di connettività intermittente e così via). Per questo motivo, potrebbe senso per mantenere i propria rappresentazione di errori, in modo che eseguire facilmente la deduplicazione. Ciò è particolarmente importante se si invia avvisi tramite posta elettronica o un gruppo equivalente.

### <a name="properties-of-faults"></a>Proprietà di errori

Questa tabella visualizza diverse proprietà chiave dell'oggetto errore. Per lo schema completo, esaminare il **MSFT\_StorageDiagnoseResult** classe *storagewmi.mof*.

| **Proprietà**              | **Esempio**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| Tipo FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Motivo                    | "Il volume è insufficiente spazio disponibile."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, Slot 11                                        |
| RecommendedActions        | {"Espandere il volume.", "Eseguire la migrazione dei carichi di lavoro per altri volumi."}   |

**FaultId** univoci all'interno dell'ambito di un cluster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {"Informativo", "Warning" e "Error"}, o equivalenti colori come blu, giallo e rosso.

**FaultingObjectDescription** parte informazioni per l'hardware, in genere vuoto per gli oggetti di software.

**FaultingObjectLocation** informazioni sulla posizione per l'hardware, in genere vuoto per gli oggetti di software.

**RecommendedActions** elenco di azioni consigliate, che sono indipendenti e senza un particolare ordine. Oggi, questo elenco è spesso di lunghezza 1.

## <a name="properties-of-fault-events"></a>Proprietà di eventi di errore

Questa tabella visualizza diverse proprietà chiave dell'evento di errore. Per lo schema completo, esaminare il **MSFT\_StorageFaultEvent** classe *storagewmi.mof*.

Nota il **ChangeType**, che indica se un errore di creazione, rimossi o aggiornati e **FaultId**. Un evento contiene anche tutte le proprietà dell'errore interessato.

| **Proprietà**              | **Esempio**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| Tipo FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Motivo                    | "Il volume è insufficiente spazio disponibile."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, Slot 11                                        |
| RecommendedActions        | {"Espandere il volume.", "Eseguire la migrazione dei carichi di lavoro per altri volumi."}   |

**ChangeType** ChangeType = {0, 1, 2} = {"creare", "Remove", "Aggiornamento"}.

## <a name="coverage"></a>Copertura

In Windows Server 2016, il servizio integrità offre i seguenti tipi di errore:  

### **<a name="physicaldisk-8"></a>Disco fisico (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>Tipo FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* Livello di gravità: avviso
* Motivo: *"il disco fisico non è riuscito."*
* RecommendedAction: *"Sostituisce il disco fisico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>Tipo FaultType: Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* Livello di gravità: avviso
* Motivo: *"Connettività è stata persa al disco fisico".*
* RecommendedAction: *"Verificare che il disco fisico sia funzioni correttamente e sia connesso correttamente".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>Tipo FaultType: Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* Livello di gravità: avviso
* Motivo: *"il disco fisico è stato rilevato ricorrente non risponde."*
* RecommendedAction: *"Sostituisce il disco fisico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>Tipo FaultType: Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* Livello di gravità: avviso
* Motivo: *"non appena si verifica è previsto un errore del disco fisico".*
* RecommendedAction: *"Sostituisce il disco fisico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>Tipo FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* Livello di gravità: avviso
* Motivo: *"il disco fisico è stato messo in quarantena perché non è supportato dal fornitore della soluzione."*
* RecommendedAction: *"Sostituire il disco fisico con componenti hardware supportati".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>Tipo FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* Livello di gravità: avviso
* Motivo: *"il disco fisico è in quarantena perché la versione del firmware non è supportata dal fornitore della soluzione."*
* RecommendedAction: *"Alla versione di destinazione, aggiornare il firmware sul disco fisico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>Tipo FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* Livello di gravità: avviso
* Motivo: *"il disco fisico ha non riconosciuto i metadati."*
* RecommendedAction: *"questo disco può contenere dati da un pool di archiviazione sconosciuto. Prima di tutto assicurarsi che non sono presenti dati utili sul disco, quindi il disco di reimpostazione."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>Tipo FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* Livello di gravità: avviso
* Motivo: *"Non riuscito tentativo di aggiornare il firmware sul disco fisico".*
* RecommendedAction: *"Prova a usare un diverso firmware binario".*

### **<a name="virtual-disk-2"></a>Disco virtuale (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>Tipo FaultType: Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* Livello di gravità: informativo
* Motivo: *"alcuni dati sul volume non sono completamente resilienti. Rimane accessibile".*
* RecommendedAction: *"Ripristino resilienza dei dati".*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>Tipo FaultType: Microsoft.Health.FaultType.VirtualDisks.Detached
* Livello di gravità: critico
* Motivo: *"il volume è inaccessibile. Alcuni dati potrebbero andare persi."*
* RecommendedAction: *"controllare fisica e/o connettività di tutti i dispositivi di archiviazione di rete. Potrebbe essere necessario eseguire il ripristino da backup."*

### **<a name="pool-capacity-1"></a>Capacità del pool (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>Tipo FaultType: Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* Livello di gravità: avviso
* Motivo: *"pool di archiviazione non ha la capacità di riserva consigliata. Ciò potrebbe limitare la possibilità di ripristinare la resilienza dei dati in caso di errori di unità."*
* RecommendedAction: *"aggiungere capacità aggiuntiva al pool di archiviazione oppure liberare capacità. Minime consigliate riserva varia in base al distribuzione, ma è pari a circa 2 unità capacità."*

### <a name="volume-capacity-2sup1sup"></a>**Capacità del volume (2)**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>Tipo FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Livello di gravità: avviso
* Motivo: *"il volume è insufficiente spazio disponibile."*
* RecommendedAction: *"Espandere il volume o la migrazione dei carichi di lavoro ad altri volumi".*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>Tipo FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Livello di gravità: critico
* Motivo: *"il volume è insufficiente spazio disponibile."*
* RecommendedAction: *"Espandere il volume o la migrazione dei carichi di lavoro ad altri volumi".*

### **<a name="server-3"></a>Server (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>Tipo FaultType: Microsoft.Health.FaultType.Server.Down
* Livello di gravità: critico
* Motivo: *"non è possibile raggiungere il server."*
* RecommendedAction: *"Start o sostituire server."*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>Tipo FaultType: Microsoft.Health.FaultType.Server.Isolated
* Livello di gravità: critico
* Motivo: *"il server è isolato dal cluster a causa di problemi di connettività".*
* RecommendedAction: *"Se isolamento persiste, controllare le reti o eseguire la migrazione dei carichi di lavoro ad altri nodi".*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>Tipo FaultType: Microsoft.Health.FaultType.Server.Quarantined
* Livello di gravità: critico
* Motivo: *"il server viene messo in quarantena dal cluster a causa di errori ricorrenti."*
* RecommendedAction: *"Sostituire il server o correggere la rete".*

### **<a name="cluster-1"></a>Cluster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>Tipo FaultType: Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* Livello di gravità: critico
* Motivo: *"il cluster è un errore del server da passare verso il basso".*
* RecommendedAction: *"controllare la risorsa di controllo e riavvia in base alle esigenze. Avviare o sostituire il server non riuscito".*

### **<a name="network-adapterinterface-4"></a>Scheda di rete/interfaccia (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>Tipo FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* Livello di gravità: avviso
* Motivo: *"è stato disconnesso l'interfaccia di rete."*
* RecommendedAction: *"Riconnettere il cavo di rete".*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>Tipo FaultType: Microsoft.Health.FaultType.NetworkInterface.Missing
* Livello di gravità: avviso
* Motivo: *"il server {server} mancano connesse alla rete di cluster {rete cluster} schede di rete."*
* RecommendedAction: *"Connettere il server alla rete cluster mancante".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>Tipo FaultType: Microsoft.Health.FaultType.NetworkAdapter.Hardware
* Livello di gravità: avviso
* Motivo: *"l'interfaccia di rete ha avuto un errore hardware".*
* RecommendedAction: *"Sostituire la scheda di interfaccia di rete".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>Tipo FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disabled
* Livello di gravità: avviso
* Motivo: *"{interfaccia di rete} l'interfaccia di rete non è abilitata e non è in uso".*
* RecommendedAction: *"abilitare l'interfaccia di rete."*

### **<a name="enclosure-6"></a>Chassis (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>Tipo FaultType: Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* Livello di gravità: avviso
* Motivo: *"Comunicazione è stata persa per enclosure di archiviazione".*
* RecommendedAction: *"Start o Sostituisci enclosure di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>Tipo FaultType: Microsoft.Health.FaultType.StorageEnclosure.FanError
* Livello di gravità: avviso
* Motivo: *"della ventola {posizione} di enclosure di archiviazione non è riuscito."*
* RecommendedAction: *"Sostituire la ventola nello chassis di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>Tipo FaultType: Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* Livello di gravità: avviso
* Motivo: *"il sensore corrente nella posizione {posizione} dell'enclosure di archiviazione non è riuscito."*
* RecommendedAction: *"Sostituire un sensore corrente nello chassis di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>Tipo FaultType: Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* Livello di gravità: avviso
* Motivo: *"il sensore di tensione nella posizione {posizione} dell'enclosure di archiviazione non è riuscito."*
* RecommendedAction: *"Sostituire un sensore tensione nello chassis di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>Tipo FaultType: Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* Livello di gravità: avviso
* Motivo: *"il controller dei / o alla posizione {posizione} di enclosure di archiviazione non è riuscito."*
* RecommendedAction: *"Sostituire un controller dei / o in enclosure di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>Tipo FaultType: Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* Livello di gravità: avviso
* Motivo: *"il sensore di temperatura nella posizione {posizione} dell'enclosure di archiviazione non è riuscito."*
* RecommendedAction: *"Sostituire un sensore di temperatura nello chassis di archiviazione".*

### **<a name="firmware-rollout-3"></a>Implementazione del firmware (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>Tipo FaultType: Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* Livello di gravità: avviso
* Motivo: *"Attualmente non è possibile l'avanzamento durante l'esecuzione di implementazione del firmware."*
* RecommendedAction: *"Verificare tutti gli spazi di archiviazione sono integri e che il dominio di errore non è attualmente in modalità di manutenzione".*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>Tipo FaultType: Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* Livello di gravità: avviso
* Motivo: *"implementazione del Firmware è stata annullata a causa di informazioni sulla versione del firmware illeggibili o imprevisto dopo l'applicazione di un aggiornamento del firmware."*
* RecommendedAction: *"riavvio firmware implementare una volta che è stato risolto il problema del firmware."*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>Tipo FaultType: Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* Livello di gravità: avviso
* Motivo: *"implementazione del Firmware è stata annullata a causa di un numero eccessivo di dischi fisici in mancanza di un tentativo di aggiornamento del firmware."*
* RecommendedAction: *"riavvio firmware implementare una volta che è stato risolto il problema del firmware."*

### <a name="storage-qos-3sup2sup"></a>**QoS di archiviazione (3)**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>Tipo FaultType: Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* Livello di gravità: avviso
* Motivo: *"velocità effettiva di archiviazione non è sufficiente per soddisfare le riserve".*
* RecommendedAction: *"Riconfigurare i criteri di QoS di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>Tipo FaultType: Microsoft.Health.FaultType.StorQos.LostCommunication
* Livello di gravità: avviso
* Motivo: *"Gestione criteri di QoS di archiviazione ha perso la comunicazione con il volume".*
* RecommendedAction: *"Riavviare nodi {nodi}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>Tipo FaultType: Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* Livello di gravità: avviso
* Motivo: *"uno o più consumatori di archiviazione (in genere macchine virtuali) siano utilizzando un criterio inesistenti con id {id}".*
* RecommendedAction: *"Ricreare eventuali criteri di QoS di archiviazione mancante".*

<sup>1</sup> indica ha raggiunto il volume completo 80% (gravità minore) o al 90% (gravità maggiore).  
<sup>2</sup> indica il minimo di IOPS per alcune unità con formato VHD sul volume non sono stati soddisfatti più 10% (minore), 30% (maggiore) o il 50% (critico) dell'intervallo di 24 ore in sequenza.  

>[!NOTE]
> L'integrità dei componenti di enclosure di archiviazione, ad esempio ventole, alimentatori e sensori deriva da servizi SES (SCSI Enclosure). Se il fornitore non specifica queste informazioni, il servizio integrità non può visualizzarlo.  

## <a name="see-also"></a>Vedere anche

- [Servizio di integrità in Windows Server 2016](health-service-overview.md)
