---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: SMB multicanale semplificato e reti di cluster Multi-NIC
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 1b9271ceac99ac9b21cbfac902ba133d66815df4
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476120"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>SMB multicanale semplificato e reti di cluster Multi-NIC

> Si applica a: Windows Server 2019, Windows Server 2016

SMB multicanale e Multi - semplificati<abbr title="scheda di interfaccia di rete">Interfaccia di rete</abbr> le reti del Cluster è una funzionalità che consente l'uso di più schede di rete nella stessa subnet di rete di cluster e abilita automaticamente SMB multicanale.

SMB multicanale semplificato e reti di Cluster Multi-NIC offre i vantaggi seguenti:  
- Clustering di failover riconosce automaticamente tutte le schede di rete nei nodi che usano la stessa opzione / stessa subnet - alcuna configurazione aggiuntiva.  
- SMB multicanale è abilitato automaticamente.  
- Le reti che hanno solo le risorse di indirizzi IP IPv6 collegamento locali (fe80) vengono riconosciute le reti (private) solo cluster.  
- Per impostazione predefinita, una singola risorsa di indirizzo IP è configurata in ogni punto di accesso del Cluster (CAP) Network Name (NN).  
- Convalida del cluster non è più generati messaggi di avviso se più schede di rete si trovano nella stessa subnet.  

## <a name="requirements"></a>Requisiti  
-   Più schede di rete per ogni server, usando la stessa opzione / subnet.  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>SMB multicanale semplificato e reti di cluster come sfruttare i vantaggi del multi-NIC  
In questa sezione descrive come sfruttare le nuove reti di cluster multi-NIC e semplificata funzionalità multicanale SMB.  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Usare almeno due reti per il Clustering di Failover   
Sebbene sia raro, commutatori di rete possono avere esito negativo, è comunque consigliabile usare almeno due reti per il Clustering di Failover. Tutte le reti che si trovano vengono usate per gli heartbeat dei cluster. Evitare di usare una singola rete per il Cluster di Failover per evitare un singolo punto di guasto. In teoria, dovrebbe esserci più percorsi fisici di comunicazione tra i nodi del cluster e nessun singolo punto di guasto.  

![Illustrazione di due reti per il Clustering di Failover](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**Figura 1: Usare almeno due reti per il Clustering di Failover**  

### <a name="use-multiple-nics-across-clusters"></a>Usare più schede di rete tra i cluster  

Il massimo vantaggio di SMB multicanale semplificato avviene quando si utilizzano più interfacce di rete tra i cluster - in sia all'archiviazione cluster di archiviazione del carico di lavoro. In questo modo i cluster di carico di lavoro (Hyper-V, Cluster di failover di SQL Server, Replica archiviazione, e così via) da utilizzare SMB multicanale e i risultati in un uso più efficiente della rete. In (chiamata anche disaggregata) convergente in cui un cluster di tipo Scale-out File Server viene usato per archiviare i dati del carico di lavoro per Hyper-V o cluster a Cluster di failover di SQL Server, questa rete viene spesso definito "subnet nord-sud" configurazione del cluster / di rete . Molti clienti ottimizzare la velocità effettiva della rete da impegnati a investire nei commutatori e le schede di interfaccia di rete con supportare per RDMA.  

![Illustrazione di una Subnet di SMB nord-sud](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**Figura 2: Per ottenere una velocità effettiva massima della rete, usare più schede di rete sia nel cluster di tipo Scale-out File Server e cluster Hyper-V o Cluster di failover di SQL Server, che condividono la subnet del Nord-Sud**  

![Schermata di due cluster con più schede di rete nella stessa subnet utilizzare SMB multicanale](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**Figura 3: Due cluster (Scale-out File Server per l'archiviazione, SQL Server <abbr title="istanza del Clustering di Failover">FCI</abbr> per carico di lavoro) utilizzano entrambi più schede di rete nella stessa subnet per utilizzare SMB multicanale e ottenere la rete migliore velocità effettiva.** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Riconoscimento automatico delle reti private IPv6 collegamento locale  
Quando vengono rilevate reti private (solo cluster) con più schede di rete, il cluster riconoscerà automaticamente gli indirizzi IP IPv6 collegamento locale (fe80) di ciascuna NIC su ciascuna subnet. Questo amministratori consente di risparmiare tempo poiché essi non più necessario configurare manualmente le risorse di indirizzo IP IPv6 collegamento locale (fe80).  

Quando si usa più di una rete privata (solo cluster), controllare la configurazione di routing IPv6 per assicurarsi che il routing non è configurato per tra subnet, poiché ciò riduce le prestazioni di rete.  

![Schermata di configurazione automatica reti nella UI di gestione Cluster di Failover](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**Figura 4: Configurazione automatica delle risorse indirizzo IPv6 Link locale (fe80)**  

## <a name="throughput-and-fault-tolerance"></a>Velocità effettiva e la tolleranza di errore  
Windows Server 2016 e Windows Server 2019 automaticamente rilevare le funzionalità di interfaccia di rete e proverà a usare ogni interfaccia di rete nella configurazione più rapida possibile. Schede di rete che sono raggruppate, schede di rete tramite RSS e schede di rete con funzionalità RDMA tutti utilizzabile. La tabella seguente riepiloga i compromessi quando si usano queste tecnologie. Velocità effettiva massima avviene quando si usano più NIC in grado di supportare RDMA. Per altre informazioni, vedere [le nozioni di base di SMB a canali multipli](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/).

![Un'illustrazione di velocità effettiva e tolleranza di errore per diverse configurazioni di interfaccia di rete](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**Figura 5: Velocità effettiva e tolleranza di errore per vari conifigurations di interfaccia di rete**   

## <a name="frequently-asked-questions"></a>Domande frequenti  
**Tutte le schede di rete in una rete multi-NIC vengono utilizzati per heartbeat del cluster?**  
    Sì.  

**Una rete multi-NIC può essere usata per solo le comunicazioni del cluster? Oppure utilizzabile solo per le comunicazioni client e cluster?**  
    Una configurazione funzionerà - tutti i ruoli di rete cluster funzionerà in una rete multi-NIC.  

**SMB multicanale è usato anche per il traffico CSV e cluster?**  
    Sì, per impostazione predefinita tutti i cluster e il traffico CSV utilizzano reti multi-NIC disponibili. Gli amministratori possono utilizzare i cmdlet di PowerShell di Clustering di Failover o gestione Cluster di Failover dell'interfaccia utente per modificare il ruolo di rete.  

**Come è possibile visualizzare le impostazioni di SMB multicanale?**  
    Usare la **Get-SMBServerConfiguration** cmdlet, cercare il valore della proprietà EnableMultiChannel.  

**È la proprietà comune cluster che plumballcrosssubnetroutes rispettato in una rete multi-NIC?**  
     Sì.  

## <a name="see-also"></a>Vedere anche  
- [What ' s New in Failover Clustering in Windows Server](whats-new-in-failover-clustering.md)  
