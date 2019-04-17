---
title: QoS di archiviazione
ms.prod: windows-server-threshold
manager: dongill
ms.author: JGerend
ms.technology: storage-qos
ms.topic: get-started-article
ms.assetid: 8dcb8cf9-0e08-4fdd-9d7e-ec577ce8d8a0
author: kumudd
ms.date: 10/10/2016
ms.openlocfilehash: 83954552b50f5f89c8b192405744836c53581d23
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="storage-quality-of-service"></a>QoS di archiviazione

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016

QoS di archiviazione (Storage Quality of Service) di Windows Server 2016 consente di monitorare e gestire centralmente le prestazioni di archiviazione delle macchine virtuali usando Hyper-V e i ruoli del File server di scalabilità orizzontale. La funzionalità migliora automaticamente l'equità delle risorse di archiviazione tra più macchine virtuali che usano lo stesso cluster di file server e consente di configurare obiettivi di prestazione minimi e massimi basati su criteri in unità di IOPS normalizzate.  

È possibile usare QoS di archiviazione in Windows Server 2016 per eseguire le operazioni seguenti:  

-   **Limitare i problemi dei vicini fastidiosi.** Per impostazione predefinita, QoS di archiviazione garantisce che una singola macchina virtuale non possa usare tutte le risorse di archiviazione e privare altre macchine virtuali della larghezza di banda di archiviazione.  

-   **Monitorare le prestazioni dell'archiviazione end-to-end.** Non appena vengono avviate le macchine virtuali archiviate in un File server di scalabilità orizzontale, le prestazioni sono monitorate. I dettagli sulle prestazioni di tutte le macchine virtuali in esecuzione e la configurazione del cluster del File server di scalabilità orizzontale possono essere visualizzate da un'unica posizione  

-   **Gestire le operazioni di I/O di archiviazione in base alle esigenze aziendali del carico di lavoro** I criteri di QoS di archiviazione definiscono i valori minimi e massimi di prestazione per le macchine virtuali e assicurano che vengano soddisfatti. Questo garantisce prestazioni coerenti delle macchine virtuali, anche in ambienti ad alta densità e con provisioning eccessivo. Se non è possibile soddisfare i criteri, sono disponibili avvisi per monitorare quando le macchine virtuali non si allineano con i criteri o i relativi criteri assegnati non sono validi.  

Questo documento descrive come le aziende possono trarre vantaggio dalla nuova funzionalità QoS di archiviazione. Si presuppone che l'utente abbia una conoscenza precedente delle nozioni di Windows Server, Clustering di failover di Windows Server, File server di scalabilità orizzontale, Hyper-V e Windows PowerShell.

## <a name="BKMK_Overview"></a>Panoramica  
Questa sezione descrive i requisiti per l'uso di QoS di archiviazione, informazioni generali su una soluzione definita da software con QoS di archiviazione e un elenco di termini correlati con QoS di archiviazione.  

### <a name="BKMK_Requirements"></a>Requisiti di QoS di archiviazione  
QoS di archiviazione supporta due scenari di distribuzione:  

-   **Hyper-V con File server di scalabilità orizzontale** Questo scenario richiede entrambi gli elementi seguenti:  

    -   Cluster di archiviazione, che è un cluster File server di scalabilità orizzontale  

    -   Cluster di calcolo, che dispone di almeno un server con il ruolo Hyper-V abilitato  

    Per QoS di archiviazione è necessario che il cluster di failover sia presente nei server di archiviazione, ma i server di calcolo non devono per forza trovarsi in un cluster di failover. Tutti i server (usati sia per l'archiviazione che per il calcolo) devono eseguire Windows Server 2016.  

    Se non si dispone di un File server di scalabilità orizzontale distribuito per scopi di valutazione, istruzioni dettagliate per crearne uno usando server esistenti o macchine virtuali sono riportate in [Windows Server 2012 R2 Storage: Step-by-step with Storage Spaces, SMB Scale-Out and Shared VHDX (Physical)](http://blogs.technet.com/b/josebda/archive/2013/07/31/windows-server-2012-r2-storage-step-by-step-with-storage-spaces-smb-scale-out-and-shared-vhdx-physical.aspx) (Archiviazione di Windows Server 2012 R2: dettagli su Spazi di archiviazione, SMB di scalabilità orizzontale e VHDX condiviso (fisico)).  

-   **Hyper-V con Volumi condivisi cluster.** Questo scenario richiede entrambi gli elementi seguenti:  

    -   Cluster di calcolo con il ruolo Hyper-V abilitato  

    -   Hyper-V con Volumi condivisi cluster per l'archiviazione  

È richiesto il cluster di failover. Tutti i server devono eseguire la stessa versione di Windows Server 2016.  

### <a name="BKMK_SolutionOverview"></a>Uso di QoS di archiviazione in una soluzione di archiviazione definita da software  
La funzionalità QoS di archiviazione è incorporata nella soluzione di archiviazione definita da software Microsoft fornita da File server di scalabilità orizzontale e Hyper-V. Il File server di scalabilità orizzontale espone le condivisioni di file ai server Hyper-V usando il protocollo SMB3. È stato aggiunto un nuovo strumento di gestione dei criteri al cluster di file server, il quale garantisce il monitoraggio delle prestazioni dell'archiviazione centrale.  

![File server di scalabilità orizzontale e QoS di archiviazione](media/overview-Clustering_SOFSStorageQoS.png)  

**Figura 1: Uso di QoS di archiviazione in una soluzione di archiviazione definita da software in File server di scalabilità orizzontale**  

Quando i server Hyper-V avviano le macchine virtuali, vengono controllati dallo strumento di gestione dei criteri. Lo strumento di gestione dei criteri comunica i criteri di QoS di archiviazione e i relativi limiti o prenotazioni al server Hyper-V, che controlla le prestazioni della macchina virtuale se necessario.  

Quando le macchine virtuali apportano modifiche ai criteri di QoS di archiviazione o alle domande di prestazioni, lo strumento di gestione dei criteri notifica al server Hyper-V di modificare il proprio comportamento. Il ciclo di feedback garantisce che tutti i dischi rigidi virtuali delle macchine virtuali operino in modo coerente secondo i criteri definiti di QoS di archiviazione.  

### <a name="BKMK_Glossary"></a>Glossario  

|Termine|Descrizione|  
|--------|---------------|  
|Operazioni di I/O normalizzate|Tutto l'uso della memoria è misurato in "Operazioni di I/O normalizzate".  Si tratta di un conteggio delle operazioni di input/output di archiviazione al secondo.  Tutte le operazioni di I/O minori a 8 KB vengono considerate come un'unica operazione di /O normalizzata.  Tutte le operazioni di I/O maggiori a 8 KB vengono considerate operazioni di I/O multiple normalizzate. Ad esempio, una richiesta di 256 KB viene considerata come 32 operazioni di I/O normalizzate.<br /><br />Windows Server 2016 include la possibilità di specificare la dimensione usata per normalizzare le operazioni di I/O.  Nel cluster di archiviazione è possibile specificare le dimensioni normalizzate che hanno effetto a livello del cluster di calcolo di normalizzazione.  Il valore predefinito rimane 8 KB.|  
|Flusso|Ogni handle di file aperto da un server Hyper-V in un file VHD o VHDX viene considerato un "flusso". Se una macchina virtuale ha due dischi rigidi virtuali collegati, disporrà di 1 flusso al cluster di server per ogni file. Se un file VHDX è condiviso con più macchine virtuali, avrà 1 flusso per ogni macchina virtuale.|  
|InitiatorName|Nome della macchina virtuale che viene segnalata al File server di scalabilità orizzontale per ogni flusso.|  
|InitiatorID|Un identificatore che corrisponde all'ID della macchina virtuale.  Può essere usato sempre per identificare i flussi singoli delle macchine virtuali anche se le macchine virtuali hanno lo stesso InitiatorName.|  
|Criteri di|I criteri di QoS di archiviazione vengono archiviati nel database del cluster e presentano le proprietà seguenti: PolicyId, MinimumIOPS, MaximumIOPS, ParentPolicy e PolicyType.|  
|PolicyId|Identificatore univoco per un criterio.  Viene generato per impostazione predefinita, ma può essere specificato se lo si preferisce.|  
|MinimumIOPS|Numero minimo di operazioni di I/O normalizzate che verrà fornito da un criterio.  Noto anche come "Prenotazione".|  
|MaximumIOPS|Numero massimo di operazioni di I/O normalizzate che verrà posto come limite da un criterio.  Noto anche come "Limite".|  
|Aggregated |Un tipo di criteri in cui i valori MinimumIOPS & MaximumIOPS specificati e la larghezza di banda vengono condivisi tra tutti i flussi assegnati al criterio. Tutti i dischi rigidi virtuali che hanno assegnato il criterio in tale sistema di archiviazione dispongono di un'allocazione singola della larghezza di banda delle operazioni di I/O da condividere.|  
|Dedicated|Un tipo di criteri in cui i valori MinimumIOPs & MaximumIOPs specificati e la larghezza di banda vengono gestiti per singoli file VHD/VHDx.|  

## <a name="BKMK_SetUpQoS"></a>Configurare QoS di archiviazione e monitorare le prestazioni di base  
Questa sezione descrive come abilitare la nuova funzionalità QoS di archiviazione e come monitorare le prestazioni di archiviazione senza applicare criteri personalizzati.  

### <a name="BKMK_SetupStorageQoSonStorageCluster"></a>Configurare QoS di archiviazione in un cluster di archiviazione  
Questa sezione descrive come abilitare QoS di archiviazione su un cluster di failover nuovo o esistente o su un file server di scalabilità orizzontale che esegue Windows Server 2016.  

#### <a name="set-up-storage-qos-on-a-new-installation"></a>Configurare QoS di archiviazione in una nuova installazione  
Se è stato configurato un nuovo cluster di failover e un volume condiviso cluster in Windows Server 2016, la funzionalità QoS di archiviazione verrà configurata automaticamente.  

#### <a name="verify-storage-qos-installation"></a>Verificare l'installazione di QoS di archiviazione  
Dopo aver creato un cluster di failover e aver configurato un disco del volume condiviso cluster, **Risorsa QoS di archiviazione** viene visualizzata come una risorsa di base del cluster ed è visibile sia in Gestione cluster di failover che in Windows PowerShell. Lo scopo è che il sistema del cluster di failover gestisca la risorsa e non sia necessario eseguire altre operazioni con questa risorsa.  Verrà visualizzata in Gestione cluster di failover e PowerShell in modo che sia coerente con le altre risorse di sistema del cluster di failover come il nuovo servizio di integrità.  

![Risorsa QoS di archiviazione visualizzata in Risorse principali del cluster](media/overview-Clustering_StorageQoSFCM.png)  

**Figura 2: Risorsa QoS di archiviazione visualizzata come una risorsa di base del cluster in Gestione cluster di failover**  

Usare il seguente cmdlet di PowerShell per visualizzare lo stato della risorsa QoS di archiviazione.  

```PowerShell  
PS C:\> Get-ClusterResource -Name "Storage Qos Resource"  

Name                   State      OwnerGroup        ResourceType                 
----                   -----      ----------        ------------                 
Storage Qos Resource   Online     Cluster Group     Storage QoS Policy Manager  
```  

### <a name="BKMK_SetupStorageQoSonComputeCluster"></a>Configurare QoS di archiviazione in un cluster di calcolo  
Il ruolo Hyper-V in Windows Server 2016 include il supporto incorporato per QoS di archiviazione e viene abilitato per impostazione predefinita.  

#### <a name="install-remote-administration-tools-to-manage-storage-qos-policies-from-remote-computers"></a>Installare gli strumenti di amministrazione remota per gestire i criteri di QoS di archiviazione da computer remoti  
È possibile gestire i criteri di QoS di archiviazione e monitorare i flussi da host di calcolo usando gli strumenti di amministrazione remota del server.  Questi strumenti sono disponibili come funzioni facoltative in tutte le installazioni di Windows Server 2016 e possono essere scaricati separatamente per Windows 10 nel sito Web [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45520).

La funzionalità facoltativa **RSAT-Clustering** include il modulo Windows PowerShell per la gestione remota del clustering di failover, inclusa QoS di archiviazione.  

-   Windows PowerShell: Add-WindowsFeature RSAT-Clustering  

La funzionalità facoltativa **RSAT-Hyper-V-Tools** include il modulo Windows PowerShell per la gestione remota di Hyper-V.  

-   Windows PowerShell: Add-WindowsFeature RSAT-Hyper-V-Tools  

#### <a name="deploy-virtual-machines-to-run-workloads-for-testing"></a>Distribuire le macchine virtuali che eseguono carichi di lavoro a scopo test  
Sarà necessario disporre di alcune macchine virtuali archiviate nel File server di scalabilità orizzontale con carichi di lavoro pertinenti.  Per alcuni suggerimenti su come simulare il carico ed eseguire alcuni test di stress, vedere la pagina seguente in cui vengono illustrati uno strumento consigliato (DiskSpd) e alcuni usi di esempio: [DiskSpd, PowerShell and storage performance: measuring IOPs, throughput and latency for both local disks and SMB file shares](http://blogs.technet.com/b/josebda/archive/2014/10/13/diskspd-powershell-and-storage-performance-measuring-iops-throughput-and-latency-for-both-local-disks-and-smb-file-shares.aspx) (DiskSpd, PowerShell e prestazioni di archiviazione: misurazione delle operazioni di I/O, della velocità effettiva e della latenza per i dischi locali e le condivisioni di file SMB).  

Gli scenari di esempio illustrati in questa guida includono cinque macchine virtuali. BuildVM1, BuildVM2, BuildVM3 e BuildVM4 eseguono un carico di lavoro desktop con domanda di archiviazione da bassa a moderata. TestVm1 esegue un benchmark di elaborazione di transazioni online con una domanda di archiviazione elevata.  

### <a name="view-current-storage-performance-metrics"></a>Visualizzare le metriche correnti relative alle prestazioni di archiviazione  
La sezione include:  

-   Eseguire una query sui flussi usando il cmdlet `Get-StorageQosFlow`.  

-   Visualizzare le prestazioni di un volume usando il cmdlet `Get-StorageQosVolume`.  

#### <a name="query-flows-using-the-get-storageqosflow-cmdlet"></a>Eseguire una query sui flussi usando il cmdlet Get-StorageQosFlow  

Il cmdlet Get-StorageQosFlow mostra tutti i flussi correnti iniziati dai server Hyper-V. Tutti i dati vengono raccolti dal cluster di File server di scalabilità orizzontale, pertanto il cmdlet può essere usato su qualsiasi nodo del cluster di File server di scalabilità orizzontale o in un server remoto usando il parametro -CimSession.  

**Il comando di esempio seguente mostra come visualizzare tutti i file aperti da Hyper-V nel server usando Get-StorageQoSFlow**.  

```PowerShell
PS C:\> Get-StorageQosFlow  

InitiatorName    InitiatorNodeNam StorageNodeName  FilePath        Status  
                 e  
-------------    ---------------- ---------------  --------        ------  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM4         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM3         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM1         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
BuildVM2         plang-c2.plan... plang-fs1.pla... C:\ClusterSt... Ok  
TR20-VMM         plang-z400.pl... plang-fs1.pla... C:\ClusterSt... Ok  
                                  plang-fs3.pla... C:\ClusterSt... Ok  
                                  plang-fs2.pla... C:\ClusterSt... Ok  
BuildVM4         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok  
BuildVM3         plang-c2.plan... plang-fs2.pla... C:\ClusterSt... Ok  
WinOltp1         plang-c1.plan... plang-fs2.pla... C:\ClusterSt... Ok  
                                  plang-fs1.pla... C:\ClusterSt... Ok  
```  

Il comando di esempio seguente viene formattato per visualizzare il nome della macchina virtuale, il nome host di Hyper-V, le operazioni di I/O e il nome file del disco rigido virtuale, ordinati in base alle operazioni di I/O.  

```PowerShell  
PS C:\> Get-StorageQosFlow | Sort-Object StorageNodeIOPs -Descending | ft InitiatorName, @{Expression={$_.InitiatorNodeName.Substring(0,$_.InitiatorNodeName.IndexOf('.'))};Label="InitiatorNodeName"}, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName InitiatorNodeName StorageNodeIOPs Status File  
------------- ----------------- --------------- ------ ----  
WinOltp1      plang-c1                     3482     Ok IOMETER.VHDX  
BuildVM2      plang-c2                      544     Ok BUILDVM2.VHDX  
BuildVM1      plang-c2                      497     Ok BUILDVM1.VHDX  
BuildVM4      plang-c2                      316     Ok BUILDVM4.VHDX  
BuildVM3      plang-c2                      296     Ok BUILDVM3.VHDX  
BuildVM4      plang-c2                      195     Ok WIN8RTM_ENTERPRISE_VL_BU...  
TR20-VMM      plang-z400                    156     Ok DATA1.VHDX  
BuildVM3      plang-c2                       81     Ok WIN8RTM_ENTERPRISE_VL_BU...  
WinOltp1      plang-c1                       65     Ok BOOT.VHDX  
                                             18     Ok DefaultFlow  
                                             12     Ok DefaultFlow  
WinOltp1      plang-c1                        4     Ok 9914.0.AMD64FRE.WINMAIN....  
TR20-VMM      plang-z400                      4     Ok DATA2.VHDX  
TR20-VMM      plang-z400                      3     Ok BOOT.VHDX  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
                                              0     Ok DefaultFlow  
```  

Il comando di esempio seguente mostra come filtrare i flussi in base a InitiatorName per trovare facilmente le prestazioni di archiviazione e le impostazioni di una macchina virtuale specifica.  

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName BuildVm1 | Format-List

FilePath           : C:\ClusterStorage\Volume2\SHARES\TWO\BUILDWORKLOAD\BUILDVM1.V  
                     HDX  
FlowId             : ebfecb54-e47a-5a2d-8ec0-0940994ff21c  
InitiatorId        : ae4e3dd0-3bde-42ef-b035-9064309e6fec  
InitiatorIOPS      : 464  
InitiatorLatency   : 26.2684  
InitiatorName      : BuildVM1  
InitiatorNodeName  : plang-c2.plang.nttest.microsoft.com  
Interval           : 300000  
Limit              : 500  
PolicyId           : b145e63a-3c9e-48a8-87f8-1dfc2abfe5f4  
Reservation        : 500  
Status             : Ok  
StorageNodeIOPS    : 475  
StorageNodeLatency : 7.9725  
StorageNodeName    : plang-fs1.plang.nttest.microsoft.com  
TimeStamp          : 2/12/2015 2:58:49 PM  
VolumeId           : 4d91fc3a-1a1e-4917-86f6-54853b2a6787  
PSComputerName     :  
MaximumIops        : 500  
MinimumIops        : 500  
```  

I dati restituiti dal cmdlet `Get-StorageQosFlow` includono:  

-   Il nome host di Hyper-V (InitiatorNodeName).  

-   Il nome della macchina virtuale e il relativo Id (InitiatorName e InitiatorId)  

-   Le prestazioni medie recenti del disco virtuale come osservato dall'host di Hyper-V (InitiatorIOPS, InitiatorLatency)  

-   Le prestazioni medie recenti del disco virtuale come osservato dal cluster di archiviazione (StorageNodeIOPS, StorageNodeLatency)  

-   I criteri attuali applicati al file, se esistenti, e la configurazione risultante (PolicyId, prenotazione, limite)  

-   Stato dei criteri  

    -   **Ok**: non sono presenti problemi  

    -   InsufficientThroughput: viene applicato un criterio ma non è possibile raggiungere il numero minimo di operazioni di I/O.  Questa situazione può verificarsi se il numero minimo per una macchina virtuale o per tutte le macchine virtuali supera quello che il volume di archiviazione può offrire.  

    -   **UnknownPolicyId**: è stato assegnato un criterio alla macchina virtuale nell'host di Hyper-V, ma questo non è presente nel file server.  Questo criterio deve essere rimosso dalla configurazione della macchina virtuale o è necessario creare un criterio corrispondente nel cluster del file server.  

#### <a name="view-performance-for-a-volume-using-get-storageqosvolume"></a>Visualizzare le prestazioni di un volume usando Get-StorageQosVolume  
Le metriche delle prestazioni di archiviazione vengono raccolte anche in base al volume di archiviazione, oltre che in base al flusso.  Ciò facilita la visualizzazione dell'uso totale medio nelle operazioni di I/O normalizzate, della latenza, dei limiti di aggregazione e delle prenotazioni applicate a un volume.  

```PowerShell
PS C:\> Get-StorageQosVolume | Format-List  

Interval       : 300000  
IOPS           : 0  
Latency        : 0  
Limit          : 0  
Reservation    : 0  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 434f561f-88ae-46c0-a152-8c6641561504  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 0  

Interval       : 300000  
IOPS           : 1097  
Latency        : 3.1524  
Limit          : 0  
Reservation    : 1568  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 4d91fc3a-1a1e-4917-86f6-54853b2a6787  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 1568  

Interval       : 300000  
IOPS           : 5354  
Latency        : 6.5084  
Limit          : 0  
Reservation    : 781  
Status         : Ok  
TimeStamp      : 2/12/2015 2:59:38 PM  
VolumeId       : 0d2fd367-8d74-4146-9934-306726913dda  
PSComputerName :  
MaximumIops    : 0  
MinimumIops    : 781  
```  

## <a name="BKMK_CreateQoSPolicies"></a>Creare e monitorare i criteri di QoS di archiviazione  
Questa sezione descrive come creare criteri di QoS di archiviazione, come applicare questi criteri alle macchine virtuali e come monitorare un cluster di archiviazione dopo l'applicazione dei criteri.  

### <a name="create-storage-qos-policies"></a>Creare i criteri di QoS di archiviazione  
I criteri di QoS di archiviazione sono definiti e gestiti nel cluster del File server di scalabilità orizzontale.  È possibile creare tutti i criteri necessari per le distribuzioni flessibili (fino a 10.000 per i cluster di archiviazione).  

Ogni file VHD/VHDX assegnato a una macchina virtuale può essere configurato con un criterio. Diversi file e macchine virtuali possono usare lo stesso criterio, oppure ognuno può essere configurato con criteri distinti.  Se più file VHD/VHDX o più macchine virtuali sono configurati con gli stessi criteri, verranno aggregati insieme e condivideranno in modo equo i valori MinimumIOPS e MaximumIOPS. Se si usano criteri distinti per più file VHD/VHDX o macchine virtuali, i valori minimo e massimo verranno registrati separatamente per ogni file o macchina.  

Se si creano più criteri simili per diverse macchine virtuali e le macchine virtuali hanno una domanda di archiviazione analoga, esse applicheranno una condivisione simile alle operazioni di I/O.  Se una macchina virtuale ha una domanda maggiore rispetto a un'altra, le operazioni di I/O seguiranno tale domanda.  

### <a name="types-of-storage-qos-policies"></a>Tipi di criteri di QoS di archiviazione  
Esistono due tipi di criteri: Aggregated (precedentemente noti come SingleInstance) e Dedicated (precedentemente noti come MultiInstance). I criteri Aggregated applicano limiti massimi e minimi per il set combinato di file VHD/VHDX e per le macchine virtuali in cui si applicano. In effetti, condividono un set specificato di operazioni di I/O e di larghezza di banda. I criteri Dedicated applicano i valori minimi e massimi separatamente per ogni file VHD/VHDx. Questo facilita la creazione di un singolo criterio che applica limiti simili a più file VHD/VHDx.  

Ad esempio, se si crea un criterio Aggregated con un minimo di 300 operazioni di I/O e un massimo di 500. Se si applica questo criterio a 5 file VHD/VHDx diversi, si è certi che i 5 file VHD/VHDx combinati garantiranno almeno 300 operazioni di I/O (se c'è domanda e il sistema di archiviazione può assicurare tali prestazioni) e non più di 500. Se i file VHD/VHDx hanno una domanda elevata simile di operazioni di I/O e il sistema di archiviazione è in grado di gestirla,ogni file VHD/VHDx otterrà circa 100 operazioni di I/O.  

Tuttavia, se si crea un criterio Dedicated con limiti simili e lo si applica ai file VHD/VHDx su 5 macchine virtuali diverse, ogni macchina virtuale otterrà almeno 300 operazioni di I/O e non più di 500. Se le macchine virtuali hanno una domanda elevata simile di operazioni di I/O e il sistema di archiviazione è in grado di gestirla,ogni macchina virtuale otterrà circa 500 operazioni di I/O. .  Se una delle macchine virtuali dispone di più file VHD/VHDx con lo stesso criterio MulitInstance configurato, esse condivideranno il limite in modo che il numero totale di operazioni di I/O della macchina virtuale dai file con tale criterio non superi i limiti.  

Di conseguenza, se si dispone di un gruppo di file VHD/VHDx che deve presentare le stesse caratteristiche di prestazioni e non si desidera creare più criteri simili, è possibile usare un singolo criterio Dedicated e applicarlo ai file di ogni macchina virtuale.  

### <a name="create-and-apply-a-dedicated-policy"></a>Creare e applicare un criterio Dedicated  
In primo luogo, usare il cmdlet `New-StorageQosPolicy` per creare un criterio nel File server di scalabilità orizzontale, come illustrato nell'esempio seguente:  

```PowerShell
$desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200  
```  

Successivamente, applicarlo ai dischi rigidi appropriati delle macchine virtuali nel server Hyper-V.  Annotare il valore PolicyId dal passaggio precedente o archiviarlo in una variabile negli script.  

Nel File server di scalabilità orizzontale creare un criterio di QoS di archiviazione usando PowerShell e ottenere il relativo ID, come illustrato nell'esempio seguente:  

```PowerShell
PS C:\> $desktopVmPolicy = New-StorageQosPolicy -Name Desktop -PolicyType Dedicated -MinimumIops 100 -MaximumIops 200  

C:\> $desktopVmPolicy.PolicyId  

Guid  
----  
cd6e6b87-fb13-492b-9103-41c6f631f8e0  
```  

Nel server Hyper-V impostare il criterio di QoS di archiviazione usando PowerShell e il relativo ID, come illustrato nell'esempio seguente:  

```PowerShell
Get-VM -Name Build* | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID cd6e6b87-fb13-492b-9103-41c6f631f8e0  
```  

### <a name="confirm-that-the-policies-are-applied"></a>Verificare l'applicazione dei criteri  
Usare il cmdlet PowerShell `Get-StorageQosFlow` per verificare che i valori MinimumIOPs e MaximumIOPs siano stati applicai ai flussi appropriati, come illustrato nell'esempio seguente.  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName |  
 ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ------ ----------- ----------- --------------- ------ ----  
BuildVM1          Ok         100         200             250     Ok BUILDVM1.VHDX  
BuildVM2          Ok         100         200             251     Ok BUILDVM2.VHDX  
BuildVM3          Ok         100         200             252     Ok BUILDVM3.VHDX  
BuildVM4          Ok         100         200             233     Ok BUILDVM4.VHDX  
TR20-VMM          Ok          33         666               1     Ok DATA2.VHDX  
TR20-VMM          Ok          33         666               5     Ok DATA1.VHDX  
TR20-VMM          Ok          33         666               4     Ok BOOT.VHDX  
WinOltp1          Ok           0           0               0     Ok 9914.0.AMD6...  
WinOltp1          Ok           0           0            5166     Ok IOMETER.VHDX  
WinOltp1          Ok           0           0               0     Ok BOOT.VHDX  
```  

Nel server Hyper-V è inoltre possibile usare lo script fornito **Get-VMHardDiskDrivePolicy.ps1** per visualizzare il criterio applicato a un'unità disco rigido virtuale.  

```PowerShell
PS C:\> Get-VM -Name BuildVM1 | Get-VMHardDiskDrive | Format-List  

Path                          : \\plang-fs.plang.nttest.microsoft.com\two\BuildWorkload  
                                \BuildVM1.vhdx  
DiskNumber                    :  
MaximumIOPS                   : 0  
MinimumIOPS                   : 0  
QoSPolicyID                   : cd6e6b87-fb13-492b-9103-41c6f631f8e0  
SupportPersistentReservations : False  
ControllerLocation            : 0  
ControllerNumber              : 0  
ControllerType                : IDE  
PoolName                      : Primordial  
Name                          : Hard Drive  
Id                            : Microsoft:AE4E3DD0-3BDE-42EF-B035-9064309E6FEC\83F8638B  
                                -8DCA-4152-9EDA-2CA8B33039B4\0\0\D  
VMId                          : ae4e3dd0-3bde-42ef-b035-9064309e6fec  
VMName                        : BuildVM1  
VMSnapshotId                  : 00000000-0000-0000-0000-000000000000  
VMSnapshotName                :  
ComputerName                  : PLANG-C2  
IsDeleted                     : False  
```  

### <a name="query-for-storage-qos-policies"></a>Eseguire una query per i criteri di QoS di archiviazione  
`Get-StorageQosPolicy` elenca tutti i criteri configurati e il relativo stato in un File server di scalabilità orizzontale.  

```PowerShell
PS C:\> Get-StorageQosPolicy  

Name                 MinimumIops          MaximumIops          Status  
----                 -----------          -----------          ------  
Default              0                    0                    Ok  
Limit500             0                    500                  Ok  
SilverVm             500                  500                  Ok  
Desktop              100                  200                  Ok  
Limit500             0                    0                    Ok  
VMM                  100                  2000                 Ok  
Vdi                  1                    100                  Ok  
```  

Lo stato può cambiare nel tempo in base alle prestazioni del sistema.  

-   **OK** - Tutti i flussi che usano tale criterio ricevono il valore MinimumIOPs richiesto.  

-   **InsufficientThroughput** - Uno o più flussi che usano tale criterio non ricevono il valore MinimumIOPs  

È inoltre possibile reindirizzare un criterio a `Get-StorageQosPolicy` per ottenere lo stato di tutti i flussi configurati, in modo da usare i criteri come indicato di seguito:  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name Desktop | Get-StorageQosFlow | ft InitiatorName, *IOPS, Status, FilePath -AutoSize  

InitiatorName MaximumIops MinimumIops InitiatorIOPS StorageNodeIOPS Status FilePat  
                                                                           h  
------------- ----------- ----------- ------------- --------------- ------ -------  
BuildVM4              100          50           187              17     Ok C:\C...  
BuildVM3              100          50           194              25     Ok C:\C...  
BuildVM1              200         100           195             196     Ok C:\C...  
BuildVM2              200         100           193             192     Ok C:\C...  
BuildVM4              200         100           187             169     Ok C:\C...  
BuildVM3              200         100           194             169     Ok C:\C...  
```  

### <a name="create-an-aggregated-policy"></a>Creare un criterio Aggregated  
I criteri Aggregated possono essere usati se si desidera che più dischi rigidi virtuali condividano un singolo pool di operazioni di I/O e la larghezza di banda.  Ad esempio, se si applica lo stesso criterio Aggregated ai dischi rigidi da due macchine virtuali, il valore minimo verrà suddiviso tra di esse in base alla domanda.  A entrambi i dischi verrà garantito un valore minimo combinato, che insieme non supererà il numero massimo di operazioni di I/O specificato o la larghezza di banda.  

Lo stesso approccio può essere usato anche per fornire una singola allocazione a tutti i file VHD/VHDx per le macchine virtuali che comprendono un servizio o che appartengono a un tenant in un ambiente con più host.  

Non esiste alcuna differenza nel processo di creazione dei criteri Dedicated e Aggregated oltre al valore PolicyType specificato.  

L'esempio seguente illustra come creare un criterio di QoS di archiviazione Aggregated e come ottenere il relativo policyID in un File server di scalabilità orizzontale:  

```PowerShell
PS C:\> $highPerf = New-StorageQosPolicy -Name SqlWorkload -MinimumIops 1000 -MaximumIops 5000 -PolicyType Aggregated  
[plang-fs]: PS C:\Users\plang\Documents> $highPerf.PolicyId  

Guid  
----  
7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

L'esempio seguente illustra come applicare i criteri di QoS di archiviazione sul server Hyper-V tramite il valore policyID ottenuto nell'esempio precedente:  

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID 7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

L'esempio seguente illustra come visualizzare gli effetti dei criteri di QoS di archiviazione dal file server:  

```PowerShell
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | format-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN  
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 1000  
MaximumIops     : 5000  
StorageNodeIOPs : 4550  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
PS C:\> Get-StorageQosFlow -InitiatorName WinOltp1 | for  
mat-list InitiatorName, PolicyId, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, FilePath  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume2\SHARES\TWO\BASEVHD\9914.0.AMD64FRE.WIN  
                  MAIN.141218-1718_SERVER_SERVERDATACENTER_EN-US.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 250  
MaximumIops     : 1250  
StorageNodeIOPs : 0  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\BOOT.VHDX  

InitiatorName   : WinOltp1  
PolicyId        : 7e2f3e73-1ae4-4710-8219-0769a4aba072  
MinimumIops     : 1000  
MaximumIops     : 5000  
StorageNodeIOPs : 4550  
FilePath        : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
```  

I valori MinimumIOPs, MaximumIOPs e MaximumIobandwidth di ogni disco rigido virtuale verrà modificato in base al relativo carico.  Ciò garantisce che la quantità totale di larghezza di banda usata per il gruppo di dischi non ecceda l'intervallo definito dai criteri.  Nell'esempio precedente, i primi due dischi sono inattivi, mentre il terzo può usare le operazioni di I/O fino al numero massimo.  Se i primi due dischi iniziano a rilasciare nuovamente operazioni di I/O, il numero massimo di operazioni di I/O del terzo disco verrà ridotto automaticamente.  

### <a name="modify-an-existing-policy"></a>Modificare un criterio esistente  
Le proprietà Name, MinimumIOPs, MaximumIOPs e MaximumIoBandwidth possono essere modificate dopo la creazione di un criterio.  Tuttavia, è impossibile modificare il tipo di criteri (Aggregated/Dedicated) dopo aver creato il criterio.  

Il cmdlet di Windows PowerShell seguente illustra come modificare la proprietà MaximumIOPs per un criterio esistente:  

```PowerShell
[DBG]: PS C:\demoscripts>> Get-StorageQosPolicy -Name SqlWorkload | Set-StorageQosPolicy -MaximumIops 6000  
```  

Il cmdlet seguente consente di verificare la modifica:  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload  

Name                    MinimumIops            MaximumIops            Status  
----                    -----------            -----------            ------  
SqlWorkload             1000                   6000                   Ok    

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageQosPolicy -Name SqlWorkload | Get-Storag  
eQosFlow | Format-Table InitiatorName, PolicyId, MaximumIops, MinimumIops, StorageNodeIops -A  
utoSize  

InitiatorName PolicyId                             MaximumIops MinimumIops StorageNodeIops  
------------- --------                             ----------- ----------- ---------------  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        1500         250               0  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072        6000        1000            4507  
```  

## <a name="BKMK_KnownIssues"></a>Identificare e risolvere i problemi comuni  
Questa sezione descrive come individuare le macchine virtuali con criteri di QoS di archiviazione non validi, come ricreare criteri corrispondenti, come rimuovere un criterio da una macchina virtuale e come identificare le macchine virtuali che non soddisfano i requisiti dei criteri di QoS di archiviazione.  

### <a name="BKMK_FindingVMsWithInvalidPolicies"></a>Identificare le macchine virtuali con criteri non validi  

Se un criterio viene eliminato dal file server prima che venga rimosso da una macchina virtuale, la macchina virtuale continuerà la sua esecuzione come se non venisse applicato alcun criterio.  

```PowerShell
PS C:\> Get-StorageQosPolicy -Name SqlWorkload | Remove-StorageQosPolicy  

Confirm  
Are you sure you want to perform this action?  
Performing the operation "DeletePolicy" on target "MSFT_StorageQoSPolicy (PolicyId =  
"7e2f3e73-1ae4-4710-8219-0769a4aba072")".  
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"):  
```  

Nello stato dei flussi verrà visualizzato "UnknownPolicyId"  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName          Status MinimumIops MaximumIops StorageNodeIOPs          Status File  
-------------          ------ ----------- ----------- ---------------          ------ ----  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0              10              Ok Def...  
                           Ok           0           0              13              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
                           Ok           0           0               0              Ok Def...  
BuildVM1                   Ok         100         200             193              Ok BUI...  
BuildVM2                   Ok         100         200             196              Ok BUI...  
BuildVM3                   Ok          50          64              17              Ok WIN...  
BuildVM3                   Ok          50         136             179              Ok BUI...  
BuildVM4                   Ok          50         100              23              Ok WIN...  
BuildVM4                   Ok         100         200             173              Ok BUI...  
TR20-VMM                   Ok          33         666               2              Ok DAT...  
TR20-VMM                   Ok          25         975               3              Ok DAT...  
TR20-VMM                   Ok          75        1025              12              Ok BOO...  
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId 991...  
WinOltp1      UnknownPolicyId           0           0            4926 UnknownPolicyId IOM...  
WinOltp1      UnknownPolicyId           0           0               0 UnknownPolicyId BOO...  
```  

#### <a name="BKMK_RecreateMatchingPolicy"></a>Ricreare un criterio di QoS di archiviazione corrispondente  
Se un criterio è stato rimosso inavvertitamente, è possibile crearne uno nuovo usando il vecchio PolicyId.  Innanzitutto, ottenere il valore PolicyId necessario  

```PowerShell
PS C:\> Get-StorageQosFlow -Status UnknownPolicyId | ft InitiatorName, PolicyId -AutoSize  

InitiatorName PolicyId  
------------- --------  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
WinOltp1      7e2f3e73-1ae4-4710-8219-0769a4aba072  
```  

Successivamente, creare un nuovo criterio usando tale valore PolicyId  

```PowerShell
PS C:\> New-StorageQosPolicy -PolicyId 7e2f3e73-1ae4-4710-8219-0769a4aba072 -PolicyType Aggregated -Name RestoredPolicy -MinimumIops 100 -MaximumIops 2000  

Name                    MinimumIops            MaximumIops            Status  
----                    -----------            -----------            ------  
RestoredPolicy          100                    2000                   Ok  
```  

Infine, verificare che il criterio sia stato applicato.  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, Status, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName Status MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ------ ----------- ----------- --------------- ------ ----  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               8     Ok DefaultFlow  
                  Ok           0           0               9     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
                  Ok           0           0               0     Ok DefaultFlow  
BuildVM1          Ok         100         200             192     Ok BUILDVM1.VHDX  
BuildVM2          Ok         100         200             193     Ok BUILDVM2.VHDX  
BuildVM3          Ok          50         100              24     Ok WIN8RTM_ENTERPRISE_VL...  
BuildVM3          Ok         100         200             166     Ok BUILDVM3.VHDX  
BuildVM4          Ok          50         100              12     Ok WIN8RTM_ENTERPRISE_VL...  
BuildVM4          Ok         100         200             178     Ok BUILDVM4.VHDX  
TR20-VMM          Ok          33         666               2     Ok DATA2.VHDX  
TR20-VMM          Ok          33         666               2     Ok DATA1.VHDX  
TR20-VMM          Ok          33         666              10     Ok BOOT.VHDX  
WinOltp1          Ok          25         500               0     Ok 9914.0.AMD64FRE.WINMA...  
```  

#### <a name="BKMK_RemovePolicyFromVM"></a>Rimuovere i criteri di QoS di archiviazione  

Se il criterio è stato rimosso intenzionalmente, o se è stata importata una macchina virtuale con un criterio non necessario, è possibile rimuovere tale criterio.  

```PowerShell
PS C:\> Get-VM -Name WinOltp1 | Get-VMHardDiskDrive | Set-VMHardDiskDrive -QoSPolicyID $null  
```  

Una volta che il valore PolicyId viene rimosso dalle impostazioni del disco rigido virtuale, lo stato sarà "Ok" e non verrà applicato nessun valore minimo o massimo.  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName MinimumIops MaximumIops StorageNodeIOPs Status File  
------------- ----------- ----------- --------------- ------ ----  
                        0           0               0     Ok DefaultFlow  
                        0           0              16     Ok DefaultFlow  
                        0           0              12     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
                        0           0               0     Ok DefaultFlow  
BuildVM1              100         200             197     Ok BUILDVM1.VHDX  
BuildVM2              100         200             192     Ok BUILDVM2.VHDX  
BuildVM3                9           9              23     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...  
BuildVM3               91         191             171     Ok BUILDVM3.VHDX  
BuildVM4                8           8              18     Ok WIN8RTM_ENTERPRISE_VL_BUILDW...  
BuildVM4               92         192             163     Ok BUILDVM4.VHDX  
TR20-VMM               33         666               2     Ok DATA2.VHDX  
TR20-VMM               33         666               1     Ok DATA1.VHDX  
TR20-VMM               33         666               5     Ok BOOT.VHDX  
WinOltp1                0           0               0     Ok 9914.0.AMD64FRE.WINMAIN.1412...  
WinOltp1                0           0            1811     Ok IOMETER.VHDX  
WinOltp1                0           0               0     Ok BOOT.VHDX  
```  

### <a name="BKMK_VMsThatDoNotMeetStorageQoSPoilicies"></a>Individuare le macchine virtuali che non rispettano i criteri di QoS di archiviazione  
Lo stato **InsufficientThroughput** è assegnato a tutti i flussi che:  

-   Dispongono di un numero minimo di operazioni di I/O definito dal criterio;  

-   Stanno avviando le operazioni di I/O a una frequenza pari o superiore al valore minimo;  

-   Non raggiungono la frequenza minima delle operazioni di I/O  

```PowerShell
PS C:\> Get-StorageQoSflow | Sort-Object InitiatorName | ft InitiatorName, MinimumIOPs, MaximumIOPs, StorageNodeIOPs, Status, @{Expression={$_.FilePath.Substring($_.FilePath.LastIndexOf('\')+1)};Label="File"} -AutoSize  

InitiatorName MinimumIops MaximumIops StorageNodeIOPs                 Status File  
------------- ----------- ----------- ---------------                 ------ ----  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0              15                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
                        0           0               0                     Ok DefaultFlow  
BuildVM3               50         100              20                     Ok WIN8RTM_ENTE...  
BuildVM3              100         200             174                     Ok BUILDVM3.VHDX  
BuildVM4               50         100              11                     Ok WIN8RTM_ENTE...  
BuildVM4              100         200             188                     Ok BUILDVM4.VHDX  
TR20-VMM               33         666               3                     Ok DATA1.VHDX  
TR20-VMM               78        1032             180                     Ok BOOT.VHDX  
TR20-VMM               22         968               4                     Ok DATA2.VHDX  
WinOltp1             3750        5000               0                     Ok 9914.0.AMD64...  
WinOltp1            15000       20000           11679 InsufficientThroughput IOMETER.VHDX  
WinOltp1             3750        5000               0                     Ok BOOT.VHDX  
```  

È possibile determinare i flussi per qualsiasi stato, tra cui **InsufficientThroughput**, come illustrato nell'esempio seguente:  

```PowerShell
PS C:\> Get-StorageQosFlow -Status InsufficientThroughput | fl  

FilePath           : C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
FlowId             : 1ca356ff-fd33-5b5d-b60a-2c8659dc803e  
InitiatorId        : 2ceabcef-2eba-4f1b-9e66-10f960b50bbf  
InitiatorIOPS      : 12168  
InitiatorLatency   : 22.983  
InitiatorName      : WinOltp1  
InitiatorNodeName  : plang-c1.plang.nttest.microsoft.com  
Interval           : 300000  
Limit              : 20000  
PolicyId           : 5d1bf221-c8f0-4368-abcf-aa139e8a7c72  
Reservation        : 15000  
Status             : InsufficientThroughput  
StorageNodeIOPS    : 12181  
StorageNodeLatency : 22.0514  
StorageNodeName    : plang-fs2.plang.nttest.microsoft.com  
TimeStamp          : 2/13/2015 12:07:30 PM  
VolumeId           : 0d2fd367-8d74-4146-9934-306726913dda  
PSComputerName     :  
MaximumIops        : 20000  
MinimumIops        : 15000  
```  

## <a name="BKMK_Health"></a>Monitorare l'integrità tramite QoS di archiviazione  
Il nuovo servizio di integrità semplifica il monitoraggio del cluster di archiviazione, offrendo un'unica posizione per cercare eventuali eventi usabili in uno dei nodi. Questa sezione descrive come monitorare l'integrità del cluster di archiviazione mediante il cmdlet `debug-storagesubsystem`.  

### <a name="view-storage-status-with-debug-storagesubsystem"></a>Visualizzare lo stato di archiviazione con Debug-StorageSubSystem  
La funzionalità Spazi di archiviazione del cluster offre inoltre informazioni sull'integrità del cluster di archiviazione in un'unica posizione. Ciò consente agli amministratori di identificare rapidamente i problemi delle distribuzioni di archiviazione e monitorare i problemi quando arrivano o vengono ignorati.  

#### <a name="vm-with-invalid-policy"></a>Macchina virtuale con criteri non validi  
Le macchine virtuali con criteri non validi vengono segnalate anche tramite il monitoraggio dell'integrità del sottosistema di archiviazione.  Di seguito è riportato un esempio dallo stesso stato, come descritto nella sezione [Identificazione delle macchine virtuali con criteri non validi](#BKMK_FindingVMsWithInvalidPolicies) di questo documento.  

```PowerShell
C:\> Get-StorageSubSystem -FriendlyName Clustered* | Debug-StorageSubSystem  

EventTime                 :  
FaultId                   : 0d16d034-9f15-4920-a305-f9852abf47c3  
FaultingObject            :  
FaultingObjectDescription : Storage QoS Policy 5d1bf221-c8f0-4368-abcf-aa139e8a7c72  
FaultingObjectLocation    :  
FaultType                 : Storage QoS policy used by consumer does not exist.  
PerceivedSeverity         : Minor  
Reason                    : One or more storage consumers (usually Virtual Machines) are  
                            using a non-existent policy with id  
                            5d1bf221-c8f0-4368-abcf-aa139e8a7c72. Consumer details:  

                            Flow ID: 1ca356ff-fd33-5b5d-b60a-2c8659dc803e  
                            Initiator ID: 2ceabcef-2eba-4f1b-9e66-10f960b50bbf  
                            Initiator Name: WinOltp1  
                            Initiator Node: plang-c1.plang.nttest.microsoft.com  
                            File Path:  
                            C:\ClusterStorage\Volume3\SHARES\THREE\WINOLTP1\IOMETER.VHDX  
RecommendedActions        : {Reconfigure the storage consumers (usually Virtual Machines)  
                            to use a valid policy., Recreate any missing Storage QoS  
                            policies.}  
PSComputerName            :  
```  

#### <a name="lost-redundancy-for-a-storage-spaces-virtual-disk"></a>Perdita di ridondanza di un disco virtuale degli spazi di archiviazione  

In questo esempio, uno spazio di archiviazione del cluster dispone di un disco virtuale creato come un mirroring a tre vie.  Un disco con errore è stato rimosso dal sistema, ma non è stato aggiunto un disco sostitutivo.  Il sottosistema di archiviazione segnala una perdita di ridondanza con HealthStatus **Avviso**, ma OperationalStatus **OK** perché il volume è ancora online.  

```PowerShell
PS C:\> Get-StorageSubSystem -FriendlyName Clustered*  

FriendlyName                   HealthStatus                   OperationalStatus  
------------                   ------------                   -----------------  
Clustered Windows Storage o... Warning                        OK  

[plang-fs1]: PS C:\Users\plang\Documents> Get-StorageSubSystem -FriendlyName Clustered* | Deb  
ug-StorageSubSystem  

EventTime                 :  
FaultId                   : dfb4b672-22a6-4229-b2ed-c29d7485bede  
FaultingObject            :  
FaultingObjectDescription : Virtual disk 'Two'  
FaultingObjectLocation    :  
FaultType                 : VirtualDiskDegradedFaultType  
PerceivedSeverity         : Minor  
Reason                    : Virtual disk 'Two' does not have enough redundancy remaining to  
                            successfully repair or regenerate its data.  
RecommendedActions        : {Rebalance the pool, replace failed physical disks, or add new  
                            physical disks to the storage pool, then repair the virtual  
                            disk.}  
PSComputerName            :  
```  

### <a name="sample-script-for-continuous-monitoring-of-storage-qos"></a>Script di esempio per il monitoraggio continuo di QoS di archiviazione  

Questa sezione include uno script di esempio che mostra come gli errori comuni possono essere monitorati usando uno script WMI.  È concepita come un punto d'inizio per gli sviluppatori per recuperare gli eventi di integrità in tempo reale.  

**Script di esempio:**  

```PowerShell
param($cimSession)  
# Register and display events  
Register-CimIndicationEvent -Namespace root\microsoft\windows\storage -ClassName msft_storagefaultevent -CimSession $cimSession  

while ($true)  
{  
     $e = (Wait-Event)  
     $e.SourceEventArgs.NewEvent  
     Remove-Event $e.SourceIdentifier  
}  
```  

## <a name="frequently-asked-questions"></a>Domande frequenti  

### <a name="how-do-i-retain-a-storage-qos-policy-being-enforced-for-my-virtual-machine-if-i-move-its-vhdvhdx-files-to-another-storage-cluster"></a>In che modo è possibile conservare un criterio di QoS di archiviazione imposto per la macchina virtuale se si spostano i file VHD/VHDx in un altro cluster di archiviazione?  

L'impostazione del file VHD/VHDx che specifica i criteri è il GUID di un ID criterio.  Quando viene creato un criterio, il GUID può essere specificato usando il parametro **PolicyID**.  Se tale parametro viene omesso, viene creato un GUID casuale.  Pertanto, è possibile ottenere il parametro PolicyID nel cluster di archiviazione in cui le macchine virtuali attualmente archiviano i file VHD/VHDx, creare un criterio identico nel cluster di archiviazione di destinazione e quindi specificare che verrà creato con lo stesso GUID.  Quando i file delle macchine virtuali vengono spostati nei nuovi cluster di archiviazione, verrà applicato il criterio con lo stesso GUID.  

System Center Virtual Machine Manager consente di applicare criteri a più cluster di archiviazione, rendendo questo scenario molto più semplice.  
### <a name="if-i-change-the-storage-qos-policy-why-dont-i-see-it-take-effect-immediately-when-i-run-get-storageqosflow"></a>Se si modifica il criterio di QoS di archiviazione, per quale motivo tale criterio non viene applicato immediatamente quando si esegue Get-StorageQoSFlow?  

Se si dispone di un flusso che raggiunge il valore massimo di un criterio e si modificano i criteri che consentono di aumentarlo o ridurlo, quindi si determina immediatamente la latenza/le operazioni di I/O/la larghezza di banda dei flussi tramite i cmdlet di PowerShell, occorreranno fino a 5 minuti per visualizzare gli effetti completi della modifica dei criteri sui flussi.  I nuovi limiti verranno applicati entro pochi secondi, ma il cmdlet di PowerShell **Get-StorgeQoSFlow** usa una media di ogni contatore tramite una finestra temporale scorrevole di 5 minuti.  In caso contrario, se è stato visualizzato un valore corrente e il cmdlet di PowerShell è stato eseguito più volte, si possono visualizzare valori molto diversi perché i valori delle operazioni di I/O e delle latenze possono variare notevolmente da un secondo all'altro.

### <a name="BKMK_Updates"></a>Quali nuove funzionalità sono state aggiunte in Windows Server 2016?

In Windows Server 2016 i tipi di criteri di QoS di archiviazione sono stati rinominati.  Il criterio **Multi-instance** è stato rinominato in **Dedicated** mentre **Single-instance** è stato rinominato in **Aggregated**. È stato inoltre modificato il comportamento di gestione dei criteri Dedicated; i file VHD/VHDX all'interno della stessa macchina virtuale che presentano lo stesso criterio **Dedicated** applicato non condivideranno le allocazioni delle operazioni di I/O.  

In Windows Server 2016 sono disponibili due nuove funzionalità QoS di archiviazione:  

-   **Larghezza di banda massima**  

    QoS di archiviazione di Windows Server 2016 introduce la possibilità di specificare la larghezza di banda massima che i flussi assegnati al criterio possono utilizzare.  Il parametro, quando specificato nei cmdlet **StorageQosPolicy**, è **MaximumIOBandwidth** e l'output viene espresso in byte al secondo.  
    Se in un criterio sono impostati i parametri **MaximimIops** e **MaximumIOBandwidth**, entrambi saranno efficaci e il primo a essere raggiunto dai flussi limiterà le operazioni di I/O dei flussi.  

-   **La normalizzazione delle operazioni di I/O è configurabile**  

    QoS di archiviazione usa la normalizzazione delle operazioni di I/O.  Il valore predefinito è una dimensione di normalizzazione di 8 KB.  QoS di archiviazione di Windows Server 2016 introduce la possibilità di specificare una dimensione di normalizzazione diversa per il cluster di archiviazione.  Questa dimensione di normalizzazione interessa tutti i flussi nel cluster di archiviazione e le relative modifiche hanno effetto immediato (entro pochi secondi).  Il valore minimo è 1 KB e il valore massimo è 4 GB (è consigliabile non impostare più di 4 MB poiché è insolito disporre di operazioni di I/O maggiori di 4 MB).  

    Un aspetto da considerare è che lo stesso modello/la stessa velocità effettiva delle operazioni di I/O viene visualizzato con un numero di operazioni di I/O diverso nell'output di QoS di archiviazione quando si modifica la normalizzazione delle operazioni di I/O, a causa di modifiche nel calcolo di normalizzazione.  Se si confrontano le operazioni di I/O tra i cluster di archiviazione è inoltre necessario verificare il valore di normalizzazione usato da ognuna, dal momento che questo influenzerà le operazioni di I/O normalizzate segnalate.    

#### <a name="example-1-creating-a-new-policy-and-viewing-the-maximum-bandwidth-on-the-storage-cluster"></a>Esempio 1: Creazione di un nuovo criterio e visualizzazione della larghezza di banda massima nel cluster di archiviazione  
In PowerShell è possibile specificare le unità con cui viene espresso un numero.  Nell'esempio seguente 10 MB viene usato come valore di larghezza di banda massimo.  QoS di archiviazione lo convertirà e lo salverà come byte al secondo. Pertanto, 10 MB viene convertito in 10485760 byte al secondo.  

```PowerShell
PS C:\Windows\system32> New-StorageQosPolicy -Name HR_VMs -MaximumIops 1000 -MinimumIops 20 -MaximumIOBandwidth 10MB  

Name   MinimumIops MaximumIops MaximumIOBandwidth Status  
----   ----------- ----------- ------------------ ------  
HR_VMs 20          1000        10485760           Ok  

PS C:\Windows\system32> Get-StorageQosPolicy  

Name    MinimumIops MaximumIops MaximumIOBandwidth Status  
----    ----------- ----------- ------------------ ------  
Default 0           0           0                  Ok  
HR_VMs  20          1000        10485760           Ok  

PS C:\Windows\system32> Get-StorageQoSFlow | fL InitiatorName,FilePath,InitiatorIOPS,InitiatorLatency,InitiatorBandwidth  

InitiatorName      : testsQoS  
FilePath           : C:\ClusterStorage\Volume2\TESTSQOS\VIRTUAL HARD DISKS\TESTSQOS.VHDX  
InitiatorIOPS      : 5  
InitiatorLatency   : 1.5455  
InitiatorBandwidth : 37888  
```  

#### <a name="example-2-get-iops-normalization-settings-and-specify--a-new-value"></a>Esempio 2: Impostazioni di normalizzazione delle operazioni di I/O e specificazione di un nuovo valore  

L'esempio seguente illustra come ottenere le impostazioni di normalizzazione delle operazioni di I/O nei cluster di archiviazione (impostazione predefinita 8 KB), quindi come impostarle su 32 KB e come visualizzarle nuovamente.  Si noti che, in questo esempio, vengono specificati "32 KB" poiché PowerShell consente di specificare l'unità anziché richiedere la conversione in byte.   L'output mostra il valore in byte al secondo.  

```PowerShell
PS C:\Windows\system32> Get-StorageQosPolicyStore  

IOPSNormalizationSize  
---------------------  
8192  

PS C:\Windows\system32> Set-StorageQosPolicyStore -IOPSNormalizationSize 32KB  
PS C:\Windows\system32> Get-StorageQosPolicyStore  

IOPSNormalizationSize  
---------------------  
32768  
```    

## <a name="see-also"></a>Vedere anche  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [Replica di archiviazione in Windows Server 2016](../storage-replica/storage-replica-overview.md)  
- [Spazi di archiviazione diretta in Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
