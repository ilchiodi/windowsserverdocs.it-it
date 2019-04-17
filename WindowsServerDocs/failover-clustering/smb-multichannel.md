---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: SMB multicanale semplificato e reti di Cluster Multi-NIC
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 45d8364adf9d3db24a8e6d8f7bc91178ce7d1551
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>SMB multicanale semplificato e reti di Cluster Multi-NIC

> Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

SMB multicanale e Multi - semplificata<abbr title="scheda di interfaccia di rete">NIC</abbr> le reti del Cluster è una nuova funzionalità di Windows Server 2016 che consente l'uso di più schede di rete nella stessa subnet di rete di cluster e abilita automaticamente SMB multicanale.  

SMB multicanale semplificato e reti di Cluster Multi-NIC offre i vantaggi seguenti:  
- Clustering di failover riconosce automaticamente tutte le NIC nei nodi che utilizzano lo stesso commutatore / stessa subnet - alcuna configurazione aggiuntiva.  
- SMB multicanale viene abilitata automaticamente.  
- Reti solo con risorse di indirizzi IP locali IPv6 collegamento (fe80) vengono riconosciute in reti (private) solo cluster.  
- Per impostazione predefinita, una singola risorsa indirizzo IP è configurata in ogni nome di rete Cluster accesso punto (PAC) (NN).  
- Convalida del cluster non è più problemi di messaggi di avviso quando vengono rilevati più schede di rete nella stessa subnet.  

## <a name="requirements"></a>Requisiti  
-   Più schede di rete per ogni server, con lo stesso commutatore / subnet.  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>Come sfruttare i vantaggi di multi-NIC cluster reti e SMB multicanale semplificato  
In questa sezione viene illustrato come sfruttare le reti di cluster multi-NIC nuove funzionalità di e semplificate SMB multicanale in Windows Server 2016.  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Utilizzare almeno due reti per il Clustering di Failover   
Sebbene sia raro, commutatori di rete possono avere esito negativo, è comunque consigliabile usare almeno due reti per il Clustering di Failover. Tutte le reti che si trovano vengono usate per heartbeat del cluster. Evita di usare una singola rete per il Cluster di Failover per evitare un singolo punto di errore. In teoria, dovrebbe esserci più percorsi fisici di comunicazione tra i nodi del cluster e alcun singolo punto di errore.  

![Illustrazione di due reti per il Clustering di Failover](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**Figura 1: Utilizzare almeno due reti per il Clustering di Failover**  

### <a name="use-multiple-nics-across-clusters"></a>Usare più schede di rete tra i cluster  

Quando vengono usati più schede di rete tra i cluster - in sia all'archiviazione cluster carico di lavoro di archiviazione, si ottiene il massimo vantaggio di SMB multicanale semplificato. In questo modo i cluster di carico di lavoro (Hyper-V, istanza del Cluster di Failover di SQL Server, Replica archiviazione, e così via) da utilizzare SMB multicanale e i risultati in un utilizzo più efficiente della rete. In (chiamata anche disaggregata) convergente in cui un cluster di File Server di scalabilità orizzontale viene utilizzato per archiviare i dati del carico di lavoro per Hyper-V o cluster di Failover Cluster istanza di SQL Server, questa rete spesso è chiamato "la subnet nord-sud" di configurazione del cluster e di rete. Molti clienti ottimizzare la velocità effettiva della rete da investire nello commutatori e schede di rete che supporti RDMA.  

![Illustrazione di una Subnet nord-sud SMB](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**Figura 2: Per ottenere una velocità effettiva massima della rete, usare più schede di rete nel cluster di File Server di scalabilità orizzontale sia il cluster Hyper-V o istanza Cluster di Failover di SQL Server - che condividono la subnet nord-sud**  

![ScreenCap di due cluster con più schede di rete nella stessa subnet per sfruttare SMB multicanale](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**Figura 3: Due cluster (File Server di scalabilità orizzontale per l'archiviazione, SQL Server <abbr title="istanza del Clustering di Failover">FCI</abbr> per carico di lavoro) utilizzano più schede di rete nella stessa subnet di utilizzare SMB multicanale e ottenere maggiore velocità effettiva della rete.** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Riconoscimento automatico delle reti private IPv6 collegamento locale  
Quando vengono rilevate reti private (solo cluster) con più schede di rete, il cluster riconoscerà automaticamente gli indirizzi IP locale IPv6 collegamento (fe80) per ogni scheda di rete in ogni subnet. Questa volta gli amministratori Salva poiché non è più necessario configurare manualmente l'indirizzo IP locale IPv6 collegamento (fe80) risorse.  

Quando si utilizza più di una rete privata (solo cluster), controllare la configurazione di routing IPv6 per assicurarsi che il routing non è configurato per croce subnet, poiché ciò riduce le prestazioni della rete.  

![ScreenCap della configurazione automatica delle reti in UI la gestione Cluster di Failover](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**Figura 4: Configurazione delle risorse automatica degli indirizzi IPv6 collegamento locale (fe80)**  

## <a name="throughput-and-fault-tolerance"></a>Velocità effettiva e la tolleranza di errore  
Windows Server 2016 automaticamente rileva funzionalità NIC e tenterà di utilizzare schede di rete nella configurazione di possibili più veloce. Schede di rete sono raggruppate, schede di rete utilizzando RSS e schede di rete con funzionalità RDMA tutti utilizzabile. Nella tabella seguente vengono riepilogati i compromessi quando si usa queste tecnologie. Velocità effettiva massima avviene quando si utilizzano schede di rete che supporti RDMA più. Per ulteriori informazioni, vedere [le nozioni di base di SMB a canali multipli](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/).

![Un'illustrazione di velocità effettiva e tolleranza di errore per diverse configurazioni NIC](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**Figura 5: La velocità effettiva e tolleranza di errore per varie NIC conifigurations**   

## <a name="frequently-asked-questions"></a>Domande frequenti  
**Tutte le NIC in una rete multi-NIC vengono utilizzate per leader cuore cluster?**  
    Sì.  

**Una rete multi-NIC può essere usata per la comunicazione solo? Oppure possono solo essere utilizzato per la comunicazione client e cluster?**  
    La configurazione funzionerà - tutti i ruoli di rete di cluster funzionerà in una rete multi-NIC.  

**SMB multicanale viene usato anche per il traffico CSV e cluster?**  
    Sì, per impostazione predefinita tutti i cluster e il traffico CSV utilizzerà reti multi-NIC disponibili. Gli amministratori possono utilizzare il cmdlet PowerShell di Clustering di Failover o gestione Cluster di Failover dell'interfaccia utente di modificare il ruolo di rete.  

**Come è possibile visualizzare le impostazioni di SMB multicanale?**  
    Utilizzare il **Get-SMBServerConfiguration** cmdlet, cercare il valore della proprietà EnableMultiChannel.  

**La proprietà comune cluster che plumballcrosssubnetroutes rispettata in una rete multi-NIC?**  
     Sì.  

## <a name="see-also"></a>Vedere anche  
- [What's New in Failover Clustering in Windows Server](whats-new-in-failover-clustering.md)  
