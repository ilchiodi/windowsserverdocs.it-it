---
title: "Servizio integrità invia report"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: bc21b9fdec5700fec23dc6af7ca15873ded34bea
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-reports"></a>Servizio integrità invia report
> Si applica a Windows Server 2016

## <a name="what-are-reports"></a>Quali sono i report?  

Il servizio integrità riduce il lavoro richiesto per ottenere informazioni sulla capacità e sulle prestazioni dal cluster spazi di archiviazione diretta. Un nuovo cmdlet offre un elenco dettagliato delle metriche essenziali, che vengono raccolte in modo efficiente e aggregate in modo dinamico tra nodi, con una logica incorporata per rilevare l'appartenenza al cluster. Tutti i valori sono solo in un momento e in tempo reale.  

## <a name="usage-in-powershell"></a>Utilizzo di PowerShell

Usare questo cmdlet per ottenere le metriche per l'intero cluster di spazi di archiviazione diretta:

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

Facoltativo **conteggio** parametro indica quanti set di valori restituire, a intervalli di un secondo.  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

È inoltre possibile ottenere le metriche per uno specifico volume o i server:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

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

Richiamare **GetReport** per avviare il flusso di esempi di un elenco scelto esperto delle metriche essenziali, che vengono raccolte in modo efficiente e aggregate in modo dinamico tra nodi, con una logica incorporata per rilevare l'appartenenza al cluster. Esempi di arriveranno in seguito ogni secondo. Tutti i valori sono solo in un momento e in tempo reale.

Per poter trasmettere le metriche per tre ambiti: il cluster, tutti i nodi o qualsiasi volume.

L'elenco completo delle metriche disponibili in ogni ambito in Windows Server 2016 è descritta di seguito.

### <a name="iobserveronnext"></a>IObserver.OnNext()

Questo codice di esempio Usa il [modello di progettazione Observer](https://msdn.microsoft.com/en-us/library/ee850490(v=vs.110).aspx) per implementare un elemento Observer cui **OnNext()** metodo viene richiamato quando arriva a ogni nuovo campione delle metriche. Il relativo **OnCompleted()** verrà chiamato se o quando lo streaming termina. Ad esempio, è possibile utilizzarla per reinizializzare la trasmissione, in modo continua per un tempo indefinito.

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

### <a name="begin-streaming"></a>Avvia lo streaming

Con il suo definito, è possibile iniziare lo streaming.

Specificare la destinazione **CimInstance** in cui si desidera le metriche di ambito. Può trattarsi di qualsiasi volume, qualsiasi nodo o il cluster.

Il parametro count è il numero di campioni prima di streaming termina.

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

Ovviamente, queste metriche possono essere rappresentate archiviate in un database o utilizzato nel modo esigenze.

### <a name="properties-of-reports"></a>Proprietà dei report

Ogni campione di metriche è un "report" che contiene i numero di record"" corrispondenti ai singoli metriche.

Per lo schema completo, esaminare il **MSFT\_StorageHealthReport** e **MSFT\_HealthRecord** le classi nello *storagewmi.mof*.

Ogni metrica ha solo tre proprietà, per questa tabella.

| **Proprietà** | **Esempio**       |
| -------------|-------------------|
| Nome         | IOLatencyAverage  |
| Valore        | 0.00021           |
| Unità        | 3                 |

Unità = {0, 1, 2, 3, 4}, dove 0 = "Byte", 1 = "BytesPerSecond", 2 = "CountPerSecond", 3 = "Seconds", o 4 = "Percentuale".

## <a name="coverage"></a>Copertura

Di seguito sono riportate le metriche disponibili per ogni ambito in Windows Server 2016.

### <a name="msftstoragesubsystem"></a>MSFT_StorageSubSystem

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


### <a name="msftstoragenode"></a>MSFT_StorageNode

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

### <a name="msftvolume"></a>MSFT_Volume

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

- [Servizio di integrità in Windows Server 2016](health-service-overview.md)
