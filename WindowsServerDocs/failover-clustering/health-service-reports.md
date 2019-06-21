---
title: Report sull'integrità del servizio
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: e018c0270a0bf410dada9c05d2c25e51fdfac1d8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280159"
---
# <a name="health-service-reports"></a>Report sull'integrità del servizio
> Si applica a: Windows Server 2019, Windows Server 2016

## <a name="what-are-reports"></a>Quali sono i report  

Il servizio integrità riduce il lavoro necessario per ottenere informazioni sulla capacità e sulle prestazioni dal cluster spazi di archiviazione diretta. Un nuovo cmdlet offre un elenco dettagliato delle metriche essenziali, che vengono raccolti in modo efficiente e aggregate in modo dinamico tra nodi, con una logica incorporata per rilevare l'appartenenza al cluster. Tutti i valori sono solo in tempo reale e temporizzati.  

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare questo cmdlet per ottenere le metriche per l'intero cluster di spazi di archiviazione diretta:

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

L'opzione facoltativa **conteggio** parametro indica quanti set di valori restituiti, a intervalli di 1 secondo.  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

È anche possibile ottenere le metriche per un volume specifico o un server:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

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

Richiamare **GetReport** per avviare lo streaming di esempi di un elenco curato esperto delle metriche essenziali, che vengono raccolti in modo efficiente e aggregate in modo dinamico tra nodi, con una logica incorporata per rilevare l'appartenenza al cluster. Esempi di arriverà successivamente ogni secondo. Tutti i valori sono solo in tempo reale e temporizzati.

Le metriche possono essere trasmessi per tre ambiti: il cluster, tutti i nodi o qualsiasi volume.

L'elenco completo di metriche disponibili in ogni ambito di Windows Server 2016 è documentato di seguito.

### <a name="iobserveronnext"></a>IObserver.OnNext()

Questo codice di esempio Usa la [schema progettuale osservatore](https://msdn.microsoft.com/library/ee850490(v=vs.110).aspx) per implementare un Observer cui **OnNext()** metodo verrà richiamato quando arriva a ogni nuovo esempio di metriche. Relativi **OnCompleted()** metodo verrà chiamato se/quando termina di streaming. Ad esempio, è possibile utilizzarla per avviare di nuovo lo streaming, in modo che continuino a tempo indeterminato.

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

### <a name="begin-streaming"></a>Avviare lo streaming

Con l'osservatore definito, è possibile avviare lo streaming.

Specificare la destinazione **CimInstance** a cui si desidera le metriche con ambite. Può trattarsi di qualsiasi volume, tutti i nodi o cluster.

Il parametro count è il numero di campioni prima entità finali di streaming.

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

Inutile a dirsi, queste metriche possono essere visualizzate, archiviate in un database o usato in qualsiasi modo desiderato.

### <a name="properties-of-reports"></a>Proprietà dei report

Ogni esempio di metriche è uno "report" che contiene molti "record" corrispondenti alle singole metriche.

Per lo schema completo, esaminare i **MSFT\_StorageHealthReport** e **MSFT\_HealthRecord** le classi nello *storagewmi.mof*.

Ogni metrica dispone semplicemente tre proprietà, per questa tabella.

| **Proprietà** | **Esempio**       |
| -------------|-------------------|
| Nome         | IOLatencyAverage  |
| Value        | 0.00021           |
| Unità di misura        | 3                 |

Unità = {0, 1, 2, 3, 4}, dove 0 = "Byte", 1 = "BytesPerSecond", 2 = "CountPerSecond", 3 = "Seconds", o 4 = "Percentage".

## <a name="coverage"></a>Ambito del servizio

Di seguito sono le metriche disponibili per ogni ambito di Windows Server 2016.

### <a name="msftstoragesubsystem"></a>MSFT_StorageSubSystem

| **Name**                        | **Unità di misura** |
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

| **Name**            | **Unità di misura** |
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

| **Name**            | **Unità di misura** |
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

- [Integrità dei servizi in Windows Server 2016](health-service-overview.md)
