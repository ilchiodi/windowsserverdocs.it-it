---
title: Report Servizio integrità
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 0a03dc5d646d24c9f24f979df36fb3fe1eafe631
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720548"
---
# <a name="health-service-reports"></a>Report Servizio integrità

> Si applica a: Windows Server 2019, Windows Server 2016

## <a name="what-are-reports"></a>Che cosa sono i report  

Il Servizio integrità riduce il lavoro necessario per ottenere informazioni sulle prestazioni e sulla capacità Live dal cluster Spazi di archiviazione diretta. Un nuovo cmdlet fornisce un elenco curato di metriche essenziali, che vengono raccolte in modo efficiente e aggregate dinamicamente tra i nodi, con la logica incorporata per rilevare l'appartenenza al cluster. Tutti i valori sono solo in tempo reale e temporizzati.  

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare questo cmdlet per ottenere le metriche per l'intero cluster Spazi di archiviazione diretta:

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

Il parametro **count** facoltativo indica il numero di set di valori da restituire, a intervalli di un secondo.  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

È anche possibile ottenere le metriche per un volume o un server specifico:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

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

Richiamare **GetReport** per iniziare lo streaming di esempi di un elenco curato da esperti di metriche essenziali, che vengono raccolte in modo efficiente e aggregate dinamicamente tra i nodi, con la logica incorporata per rilevare l'appartenenza al cluster. Gli esempi saranno ricevuti ogni secondo in seguito. Tutti i valori sono solo in tempo reale e temporizzati.

È possibile trasmettere le metriche per tre ambiti, ovvero il cluster, qualsiasi nodo o qualsiasi volume.

L'elenco completo delle metriche disponibili in ogni ambito di Windows Server 2016 è illustrato di seguito.

### <a name="iobserveronnext"></a>IObserver. OnNext ()

Questo codice di esempio usa il [modello di progettazione Observer](https://msdn.microsoft.com/library/ee850490(v=vs.110).aspx) per implementare un Observer il cui metodo **OnNext ()** verrà richiamato ogni volta che arriva un nuovo campione di metriche. Il metodo **OnCompleted ()** verrà chiamato se/al termine del flusso. Ad esempio, è possibile usarlo per reinizializzare il flusso, in modo che continui a tempo indefinito.

```
class MetricsObserver<T> : IObserver<T>
{
    public void OnNext(T Result)
    {
        // Cast
        CimMethodStreamedResult StreamedResult = Result as CimMethodStreamedResult;

        if (StreamedResult != null)
        {
            // For illustration, you could store the metrics in this dictionary
            Dictionary<string, string> Metrics = new Dictionary<string, string>();

            // Unpack
            CimInstance Report = (CimInstance)StreamedResult.ItemValue;
            IEnumerable<CimInstance> Records = (IEnumerable<CimInstance>)Report.CimInstanceProperties["Records"].Value;
            foreach (CimInstance Record in Records)
            {
                /// Each Record has "Name", "Value", and "Units"
                Metrics.Add(
                    Record.CimInstanceProperties["Name"].Value.ToString(),
                    Record.CimInstanceProperties["Value"].Value.ToString()
                    );
            }

            // TODO: Whatever you want!
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Reinvoke BeginStreamingMetrics(), defined in the next section
    }
}
```

### <a name="begin-streaming"></a>Inizia streaming

Con l'Observer definito, è possibile iniziare la trasmissione.

Specificare il **CimInstance** di destinazione a cui si desidera applicare l'ambito delle metriche. Può essere il cluster, qualsiasi nodo o qualsiasi volume.

Il parametro count è il numero di campioni prima della fine del flusso.

```
CimInstance Target = Cluster; // From among the objects discovered in DiscoverObjects()

public void BeginStreamingMetrics(CimSession Session, CimInstance HealthService, CimInstance Target)
{
    // Set Parameters
    CimMethodParametersCollection MetricsParams = new CimMethodParametersCollection();
    MetricsParams.Add(CimMethodParameter.Create("TargetObject", Target, CimType.Instance, CimFlags.In));
    MetricsParams.Add(CimMethodParameter.Create("Count", 999, CimType.UInt32, CimFlags.In));
    // Enable WMI Streaming
    CimOperationOptions Options = new CimOperationOptions();
    Options.EnableMethodResultStreaming = true;
    // Invoke API
    CimAsyncMultipleResults<CimMethodResultBase> InvokeHandler;
    InvokeHandler = Session.InvokeMethodAsync(
        HealthService.CimSystemProperties.Namespace, HealthService, "GetReport", MetricsParams, Options
        );
    // Subscribe the Observer
    MetricsObserver<CimMethodResultBase> Observer = new MetricsObserver<CimMethodResultBase>(this);
    IDisposable Disposeable = InvokeHandler.Subscribe(Observer);
}
```

Inutile dirlo, queste metriche possono essere visualizzate, archiviate in un database o usate nel modo appropriato.

### <a name="properties-of-reports"></a>Proprietà dei report

Ogni esempio di metrica è un "report" che contiene molti "record" corrispondenti a singole metriche.

Per lo schema completo, esaminare le **classi\_MSFT StorageHealthReport** e **MSFT\_HealthRecord** in *StorageWMI. mof*.

Ogni metrica dispone solo di tre proprietà, in base a questa tabella.

| **Proprietà** | **Esempio**       |
| -------------|-------------------|
| Nome         | IOLatencyAverage  |
| valore        | 0,00021           |
| Unità        | 3                 |

Unità = {0, 1, 2, 3, 4}, dove 0 = "byte", 1 = "BytesPerSecond", 2 = "CountPerSecond", 3 = "secondi" o 4 = "percentuale".

## <a name="coverage"></a>Copertura

Di seguito sono riportate le metriche disponibili per ogni ambito di Windows Server 2016.

### <a name="msft_storagesubsystem"></a>MSFT_StorageSubSystem

| **Nome**                        | **Unità** |
|---------------------------------|-----------|
| CPUUsage                        | 4         |
| CapacityPhysicalPooledAvailable | 0         |
| CapacityPhysicalPooledTotal     | 0         |
| CapacityPhysicalTotal           | 0         |
| CapacityPhysicalUnpooled        | 0         |
| CapacityVolumesAvailable        | 0         |
| CapacityVolumesTotal            | 0         |
| IOLatencyAverage                | 3         |
| IOLatencyRead                   | 3         |
| IOLatencyWrite                  | 3         |
| IOPSRead                        | 2         |
| IOPSTotal                       | 2         |
| IOPSWrite                       | 2         |
| IOThroughputRead                | 1         |
| IOThroughputTotal               | 1         |
| IOThroughputWrite               | 1         |
| MemoryAvailable                 | 0         |
| MemoryTotal                     | 0         |


### <a name="msft_storagenode"></a>MSFT_StorageNode

| **Nome**            | **Unità** |
|---------------------|-----------|
| CPUUsage            | 4         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |
| MemoryAvailable     | 0         |
| MemoryTotal         | 0         |

### <a name="msft_volume"></a>MSFT_Volume

| **Nome**            | **Unità** |
|---------------------|-----------|
| CapacityAvailable   | 0         |
| CapacityTotal       | 0         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |

## <a name="see-also"></a>Vedere anche

- [Servizio integrità in Windows Server 2016](health-service-overview.md)
