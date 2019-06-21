---
title: Errori del servizio integrità
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 72b1593503db75aa275b9eb45c8342cee6724001
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280393"
---
# <a name="health-service-faults"></a>Errori del servizio integrità
> Si applica a: Windows Server 2019, Windows Server 2016

## <a name="what-are-faults"></a>Quali sono gli errori

Il servizio integrità controlla costantemente il cluster di spazi di archiviazione diretta per rilevare i problemi e generare "errori". Un nuovo cmdlet Visualizza tutti gli errori correnti, consentendo di verificare l'integrità della distribuzione senza esaminare ogni singola entità o di funzionalità a sua volta con facilità. Gli errori sono progettati per essere precisi, facili da comprendere e correggibili.  

Ogni errore contiene cinque campi importanti:  

-   Severity
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
 > La posizione fisica viene derivata dalla configurazione del dominio di errore. Per altre informazioni sui domini di errore, vedere [domini di errore in Windows Server 2016](fault-domains.md). Se non si specifica questa informazione, il campo della posizione sarà meno utile, ad esempio potrebbe visualizzare solo il numero di slot.  

## <a name="root-cause-analysis"></a>Analisi della causa radice

Il servizio integrità può valutare la causalità potenziale tra eventi di errore entità per identificare e combinare gli errori causati dello stesso problema sottostante. Riconoscendo gli effetti concatenati, è possibile generare report più concisi. Ad esempio, se un server è inattivo, è previsto rispetto a tutte le unità all'interno del server saranno anche senza la connettività. Pertanto, verrà generato un solo errore relativo alla causa principale, in questo caso, il server.  

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Per visualizzare gli errori correnti in PowerShell, eseguire questo cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

Restituisce tutti gli errori relativi al cluster di spazi di archiviazione diretta complessivo. In genere, questi errori riguardano all'hardware o configurazione. Se non vengono rilevati errori, questo cmdlet restituisce nothing.  

>[!NOTE]
> In un ambiente non di produzione e a proprio rischio, è possibile sperimentare questa funzionalità generando errori manualmente, ad esempio, la rimozione di un disco fisico o arrestando un nodo. Una volta è apparsa l'errore, reinserire il disco fisico oppure riavviare che il nodo e l'errore non verrà più visualizzato anche in questo caso.

È anche possibile visualizzare gli errori che interessano solo condivisioni file con i cmdlet seguenti o volumi specifici:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Restituisce tutti gli errori che interessano solo la specifica condivisione di file o volumi. In genere, questi errori riguardano la pianificazione della capacità, la resilienza dei dati o funzionalità, ad esempio Storage Quality of Service o la Replica di archiviazione. 

## <a name="usage-in-net-and-c"></a>Utilizzo in .NET eC#

### <a name="connect"></a>Connetti

Per eseguire query sul servizio di integrità, è necessario stabilire una **CimSession** con il cluster. A tale scopo, sono necessari alcuni elementi che sono disponibili solo in .NET completo, vale a dire non è possibile facilmente eseguire questa operazione direttamente da un'app per dispositivi mobili o web. Questi esempi di codice userà C\#il sistema più semplice scelta per questo livello di accesso ai dati.

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

Il nome utente fornito deve essere un amministratore locale del Computer di destinazione.

È consigliabile che la Password costruire **SecureString** direttamente dall'input dell'utente in tempo reale, pertanto la propria password non viene mai archiviato in memoria come testo non crittografato. Ciò consente di attenuare un'ampia gamma di problemi di sicurezza. Ma in pratica, è comune a scopo di creazione di prototipi di costruzione, come illustrato in precedenza.

### <a name="discover-objects"></a>Individuazione degli oggetti

Con il **CimSession** stabilito, è possibile eseguire una query Strumentazione gestione Windows (WMI) nel cluster.

Prima di poter ottenere errori o metriche, è necessario ottenere le istanze di diversi oggetti pertinenti. Prima di tutto, il **MSFT\_StorageSubSystem** che rappresenta gli spazi di archiviazione diretta nel cluster. Che usa, è possibile ottenere ogni **MSFT\_StorageNode** nel cluster e ogni **MSFT\_Volume**, i volumi di dati. Infine, è necessario il **MSFT\_StorageHealth**, lo stato di integrità del servizio stesso, troppo.

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

Questi sono gli stessi oggetti è visualizzato in PowerShell usando i cmdlet, ad esempio **Get-StorageSubSystem**, **Get-StorageNode**, e **Get-Volume**.

È possibile accedere le stesse proprietà, documentata in [classi API di gestione archiviazione](https://msdn.microsoft.com/library/windows/desktop/hh830612(v=vs.85).aspx).

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

Richiamare **diagnosticare** per ottenere tutti gli errori correnti nell'ambito di destinazione **CimInstance**, che essere cluster o qualsiasi volume.

L'elenco completo degli errori disponibili in ogni ambito di Windows Server 2016 è documentato di seguito.

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

### <a name="optional-myfault-class"></a>Facoltativo: Classe MyFault

Si potrebbe avere senso per costruire e rendere persistente il proprio rappresentazione degli errori. Ad esempio, ciò **MyFault** classe archivia diverse proprietà chiave di errori, inclusi i **FaultId**, che può essere usato in un secondo momento per associare l'aggiornamento o rimuovere le notifiche o deduplicare nel caso in cui l'errore stesso è stato rilevato più volte, per qualsiasi motivo.

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

L'elenco completo delle proprietà in ogni errore (**DiagnoseResult**) è documentato di seguito.

### <a name="fault-events"></a>Eventi di errore

Quando gli errori vengono creati, rimossi o aggiornati, il servizio integrità genera gli eventi WMI. Questi sono essenziali per mantenere sincronizzati senza frequenti polling dello stato dell'applicazione e può aiutare con prodotti quali determinazione della necessità di inviare avvisi di posta elettronica, ad esempio. Per sottoscrivere questi eventi, questo codice di esempio Usa nuovamente lo schema progettuale osservatore.

In primo luogo, sottoscrivere **MSFT\_StorageFaultEvent** gli eventi.

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

Successivamente, implementare un Observer cui **OnNext()** metodo verrà richiamato ogni volta che viene generato un nuovo evento.

Ogni evento contiene **ChangeType** che indica se un errore viene creato, aggiornato o rimosso e il relativo **FaultId**.

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

### <a name="understand-fault-lifecycle"></a>Comprensione del ciclo di vita di errore

Gli errori non devono essere contrassegnate come "visualizzazione" o risolto dall'utente. Vengono create quando il servizio integrità rileva un problema e vengono rimossi automaticamente e solo quando il servizio integrità non sarà più possibile osservare il problema. In generale, indica che è stato risolto il problema.

Tuttavia, in alcuni casi, gli errori potrebbero essere rilevato più volte perché il servizio integrità (ad esempio, dopo il failover, oppure a causa di errori di connettività intermittenti e così via). Per questo motivo, potrebbe avrebbe senso per rendere persistente il proprio rappresentazione degli errori, in modo che è facilmente possibile deduplicare. Ciò è particolarmente importante se si invia avvisi tramite posta elettronica o equivalente.

### <a name="properties-of-faults"></a>Proprietà degli errori

Questa tabella presenta diverse proprietà chiave dell'oggetto errore. Per lo schema completo, esaminare i **MSFT\_StorageDiagnoseResult** classe *storagewmi.mof*.

| **Proprietà**              | **Esempio**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| `Reason`                    | "Il volume è quasi esaurito lo spazio disponibile."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Installare in rack A06, UR 25, lo Slot 11                                        |
| RecommendedActions        | {"Espandere il volume.", "Eseguire la migrazione dei carichi di lavoro per altri volumi."}   |

**FaultId** univoco all'interno dell'ambito di un cluster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {"Informational", "Warning" e "Error"}, o equivalenti colori, ad esempio blue, giallo e rosso.

**FaultingObjectDescription** parte informazioni per l'hardware, in genere vuoto per gli oggetti software.

**FaultingObjectLocation** informazioni sulla posizione per l'hardware, in genere vuoto per gli oggetti software.

**RecommendedActions** elenco di azioni consigliate, che sono indipendenti e in nessun ordine particolare. Attualmente, questo elenco è spesso di lunghezza 1.

## <a name="properties-of-fault-events"></a>Proprietà degli eventi di errore

Questa tabella presenta diverse proprietà chiave dell'evento di errore. Per lo schema completo, esaminare i **MSFT\_StorageFaultEvent** classe *storagewmi.mof*.

Si noti il **ChangeType**, che indica se viene creato, un errore, rimossi o aggiornati e il **FaultId**. Un evento contiene anche tutte le proprietà dell'errore interessato.

| **Proprietà**              | **Esempio**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| `Reason`                    | "Il volume è quasi esaurito lo spazio disponibile."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Installare in rack A06, UR 25, lo Slot 11                                        |
| RecommendedActions        | {"Espandere il volume.", "Eseguire la migrazione dei carichi di lavoro per altri volumi."}   |

**ChangeType** ChangeType = { 0, 1, 2 } = { "Create", "Remove", "Update" }.

## <a name="coverage"></a>Ambito del servizio

In Windows Server 2016, il servizio integrità offre i tipi di errore seguenti:  

### <a name="physicaldisk-8"></a>**PhysicalDisk (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* Gravità: Avviso
* Motivo: *"Il disco fisico è non riuscito".*
* RecommendedAction: *"Sostituire il disco fisico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* Gravità: Avviso
* Motivo: *"Connettività persa al disco fisico."*
* RecommendedAction: *"Verificare che il disco fisico sia funzionante e connessi correttamente".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* Gravità: Avviso
* Motivo: *"Il disco fisico è stato rilevato il blocco ricorrente."*
* RecommendedAction: *"Sostituire il disco fisico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* Gravità: Avviso
* Motivo: *"Errore del disco fisico, secondo la stima si verificano a breve."*
* RecommendedAction: *"Sostituire il disco fisico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* Gravità: Avviso
* Motivo: *"Il disco fisico viene messo in quarantena perché non è supportata per il fornitore della soluzione."*
* RecommendedAction: *"Sostituire il disco fisico con hardware supportato."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* Gravità: Avviso
* Motivo: *"Il disco fisico è in quarantena perché la versione del firmware non è supportata per il fornitore della soluzione".*
* RecommendedAction: *"Aggiornamento del firmware del disco fisico per la versione di destinazione".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* Gravità: Avviso
* Motivo: *"Il disco fisico ha dei metadati non riconosciuto".*
* RecommendedAction: *"Questo disco può contenere i dati da un pool di archiviazione sconosciuto. Prima di tutto verificare che non sono presenti dati utili sul disco, quindi reimpostare il disco."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* Gravità: Avviso
* Motivo: *"Tentativo non riuscito per l'aggiornamento del firmware del disco fisico."*
* RecommendedAction: *"Prova con una diversa del firmware binary."*

### <a name="virtual-disk-2"></a>**Virtual Disk (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* Gravità: Informativo
* Motivo: *"Alcuni dati sul volume non completamente resilienti. Rimane accessibile."*
* RecommendedAction: *"Ripristino della resilienza dei dati".*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.Detached
* Gravità: Critico
* Motivo: *"Il volume non è accessibile. Alcuni dati potrebbero essere persi".*
* RecommendedAction: *"Controllo fisico e/o la connettività di tutti i dispositivi di archiviazione di rete. Potrebbe essere necessario ripristinare dal backup."*

### <a name="pool-capacity-1"></a>**Capacità del pool (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType: Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* Gravità: Avviso
* Motivo: *"Il pool di archiviazione non è la capacità di riserva consigliato. Tale condizione può limitare la possibilità di ripristinare la resilienza dei dati in caso di errori di unità."*
* RecommendedAction: *"Aggiungere ulteriore capacità per il pool di archiviazione oppure liberare la capacità. Il valore minimo consigliato riserva varia con la distribuzione, ma è il patrimonio di circa 2 unità di capacità."*

### <a name="volume-capacity-2sup1sup"></a>**Capacità del volume (2)** <sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Gravità: Avviso
* Motivo: *"Il volume è quasi esaurito lo spazio disponibile."*
* RecommendedAction: *"Espandere il volume o eseguire la migrazione dei carichi di lavoro in altri volumi."*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Gravità: Critico
* Motivo: *"Il volume è quasi esaurito lo spazio disponibile."*
* RecommendedAction: *"Espandere il volume o eseguire la migrazione dei carichi di lavoro in altri volumi."*

### <a name="server-3"></a>**Server (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType: Microsoft.Health.FaultType.Server.Down
* Gravità: Critico
* Motivo: *"Il server non può essere raggiunto".*
* RecommendedAction: *"Start o sostituire server".*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType: Microsoft.Health.FaultType.Server.Isolated
* Gravità: Critico
* Motivo: *"Il server è isolato da cluster a causa di problemi di connettività."*
* RecommendedAction: *"Se isolamento persiste, controllare le reti o eseguire la migrazione dei carichi di lavoro ad altri nodi."*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType: Microsoft.Health.FaultType.Server.Quarantined
* Gravità: Critico
* Motivo: *"Il server viene messo in quarantena da parte del cluster a causa di errori ricorrenti."*
* RecommendedAction: *"Sostituire il server o risolvere la rete".*

### <a name="cluster-1"></a>**Cluster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType: Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* Gravità: Critico
* Motivo: *"Il cluster è un errore del server dall'indisponibilità".*
* RecommendedAction: *"Verificare la risorsa di controllo del mirroring e riavviare in base alle esigenze. Avviare o sostituire i server non riusciti".*

### <a name="network-adapterinterface-4"></a>**Network Adapter/Interface (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* Gravità: Avviso
* Motivo: *"È stato disconnesso l'interfaccia di rete."*
* RecommendedAction: *"Riconnettere il cavo di rete".*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType: Microsoft.Health.FaultType.NetworkInterface.Missing
* Gravità: Avviso
* Motivo: *"Il server {server} ha mancante connesso alla rete di cluster {rete cluster} schede di rete".*
* RecommendedAction: *"Connessione al server per la rete di cluster mancanti".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Hardware
* Gravità: Avviso
* Motivo: *"L'interfaccia di rete ha avuto un guasto hardware".*
* RecommendedAction: *"Sostituire la scheda di interfaccia di rete".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disabled
* Gravità: Avviso
* Motivo: *"L'interfaccia di rete {interfaccia di rete} non è abilitato e non è in uso."*
* RecommendedAction: *"Abilita l'interfaccia di rete".*

### <a name="enclosure-6"></a>**Enclosure (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* Gravità: Avviso
* Motivo: *"La comunicazione è stata persa a enclosure di archiviazione".*
* RecommendedAction: *"Start o sostituire lo chassis di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.FanError
* Gravità: Avviso
* Motivo: *"La ventola dalla posizione {posizione} di enclosure di archiviazione non è riuscita."*
* RecommendedAction: *"Sostituire il fan-in dell'enclosure di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* Gravità: Avviso
* Motivo: *"Il sensore corrente nella posizione {posizione} dell'enclosure di archiviazione non è riuscita."*
* RecommendedAction: *"Sostituire un sensore corrente nello chassis di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* Gravità: Avviso
* Motivo: *"Il sensore di tensione dalla posizione {posizione} di enclosure di archiviazione non è riuscita."*
* RecommendedAction: *"Sostituire un sensore di tensione nello chassis di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* Gravità: Avviso
* Motivo: *"Il controller dei / o alla posizione {posizione} di enclosure di archiviazione non è riuscita."*
* RecommendedAction: *"Sostituire un controller dei / o nello chassis di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* Gravità: Avviso
* Motivo: *"Il sensore di temperatura dalla posizione {posizione} di enclosure di archiviazione non è riuscita."*
* RecommendedAction: *"Sostituire un sensore di temperatura nello chassis di archiviazione".*

### <a name="firmware-rollout-3"></a>**Firmware Rollout (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* Gravità: Avviso
* Motivo: *"Attualmente non è possibile fare progressi durante l'esecuzione di firmware di rollout."*
* RecommendedAction: *"Verificare tutti gli spazi di archiviazione siano integri e che nessun dominio di errore è attualmente in modalità di manutenzione".*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* Gravità: Avviso
* Motivo: *"Implementazione del firmware è stato annullato a causa di informazioni sulla versione del firmware non leggibile o non previsto dopo l'applicazione di un aggiornamento del firmware."*
* RecommendedAction: *"Restart firmware il rollout dopo che è stato risolto il problema del firmware."*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* Gravità: Avviso
* Motivo: *"Implementazione del firmware è stato annullato a causa di troppi dischi fisici in mancanza di un tentativo di aggiornamento del firmware."*
* RecommendedAction: *"Restart firmware il rollout dopo che è stato risolto il problema del firmware."*

### <a name="storage-qos-3sup2sup"></a>**QoS di archiviazione (3)** <sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType: Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* Gravità: Avviso
* Motivo: *"Velocità effettiva di archiviazione è insufficiente per soddisfare le riserve".*
* RecommendedAction: *"Riconfigurare i criteri QoS di archiviazione".*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorQos.LostCommunication
* Gravità: Avviso
* Motivo: *"Il gestore dei criteri QoS di archiviazione ha perso la comunicazione con il volume".*
* RecommendedAction: *". Riavviare i nodi {nodi}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType: Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* Gravità: Avviso
* Motivo: *"Uno o più consumer di archiviazione (in genere macchine virtuali) sta utilizzando un criterio non esistente con id {id}".*
* RecommendedAction: *"Ricreare tutti i criteri QoS di archiviazione mancanti".*

<sup>1</sup> indica il volume ha raggiunto all'80% (gravità minore) o al 90% (gravità maggiore).  
<sup>2</sup> indica il numero minimo di IOPS per alcune unità con formato VHD sul volume non sono state soddisfatte su 10% (minore), 30% (maggiore) o il 50% (critico) dell'arco della finestra di 24 ore.  

>[!NOTE]
> L'integrità dei componenti dell’alloggiamento di archiviazione, ad esempio ventole, alimentatori e sensori deriva da servizi SES (SCSI Enclosure Services). Se il fornitore non specifica queste informazioni, Servizio integrità non può visualizzarlo.  

## <a name="see-also"></a>Vedere anche

- [Integrità dei servizi in Windows Server 2016](health-service-overview.md)
