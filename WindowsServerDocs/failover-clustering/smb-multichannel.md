---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: SMB multicanale semplificato e reti di cluster Multi-NIC
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 7816016daae1d06568cd6149791a9a368d8602f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361108"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>SMB multicanale semplificato e reti di cluster Multi-NIC

> Si applica a: Windows Server 2019, Windows Server 2016

SMB multicanale e multicanale semplificato<abbr title="Scheda di interfaccia di rete">NIC</abbr> Le reti cluster sono una funzionalità che consente l'utilizzo di più schede di rete nella stessa subnet di rete cluster e Abilita automaticamente SMB multicanale.

Le reti SMB multicanale e multicanale semplificate offrono i vantaggi seguenti:  
- Il clustering di failover riconosce automaticamente tutte le schede di rete nei nodi che usano lo stesso switch/stessa subnet. non è necessaria alcuna configurazione aggiuntiva.  
- SMB multicanale è abilitato automaticamente.  
- Le reti che dispongono solo di indirizzi IP FE80 (IPv6 link local) sono riconosciute in reti solo cluster (private).  
- Per impostazione predefinita, viene configurata una sola risorsa indirizzo IP per ogni nome di rete del punto di accesso cluster (CAP).  
- La convalida del cluster non emette più messaggi di avviso quando vengono trovate più schede di rete nella stessa subnet.  

## <a name="requirements"></a>Requisiti  
-   Più schede di rete per server, usando la stessa opzione/subnet.  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>Come sfruttare i vantaggi delle reti di cluster a più NIC e SMB multicanale semplificato  
Questa sezione descrive come sfruttare i vantaggi delle nuove reti di cluster MultiNIC e le funzionalità semplificate SMB multicanale.  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Usare almeno due reti per il clustering di failover   
Sebbene sia raro, i commutatori di rete possono avere esito negativo. è comunque consigliabile utilizzare almeno due reti per il clustering di failover. Tutte le reti trovate vengono usate per gli heartbeat del cluster. Evitare di utilizzare una singola rete per il cluster di failover per evitare un singolo punto di errore. Idealmente, dovrebbero essere presenti più percorsi di comunicazione fisica tra i nodi del cluster e nessun singolo punto di errore.  

@no__t 0Illustration di due reti per il clustering di failover @ no__t-1  
**Figura 1: Usare almeno due reti per il clustering di failover @ no__t-0  

### <a name="use-multiple-nics-across-clusters"></a>Usare più NIC tra cluster  

Il vantaggio massimo del multicanale SMB semplificato viene eseguito quando vengono usate più schede di rete tra i cluster, sia nei cluster dei carichi di lavoro di archiviazione che in quelli di archiviazione. Ciò consente ai cluster del carico di lavoro (Hyper-V, SQL Server istanza del cluster di failover, replica di archiviazione e così via) di usare SMB multicanale e di ottenere un uso più efficiente della rete. In una configurazione di cluster convergente, nota anche come disaggregata, in cui un cluster di file server di scalabilità orizzontale viene usato per l'archiviazione dei dati del carico di lavoro per un cluster di istanze del cluster di failover Hyper-V o SQL Server, questa rete viene spesso denominata "subnet Nord-Sud"/rete . Molti clienti massimizzano la velocità effettiva di questa rete investendo in RDMA schede NIC in grado di supportare e commutatori.  

@no__t 0Illustration di una subnet SMB nord-sud @ no__t-1  
**Figura 2: Per ottenere la massima velocità effettiva della rete, usare più schede di rete nel cluster di file server di scalabilità orizzontale e nel cluster dell'istanza del cluster di failover Hyper-V o SQL Server, che condividono la subnet nord-sud @ no__t-0  

@no__t 0Screencap di due cluster che usano più schede di rete nella stessa subnet per sfruttare SMB multicanale @ no__t-1  
**Figura 3: Due cluster (file server di scalabilità orizzontale per l'archiviazione, SQL Server istanza di clustering <abbr title="Failover @ no__t-1FCI @ no__t-2 per il carico di lavoro) usano più NIC nella stessa subnet per sfruttare SMB multicanale e ottenere una migliore velocità effettiva della rete.** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Riconoscimento automatico di reti private locali con collegamento IPv6  
Quando vengono rilevate reti private (solo cluster) con più schede di interfaccia di rete, il cluster rileverà automaticamente gli indirizzi IP FE80 (IPv6 link local) per ogni scheda di interfaccia di rete in ogni subnet. Questo consente di risparmiare tempo per gli amministratori, perché non è più necessario configurare manualmente le risorse degli indirizzi IP (FE80) del collegamento IPv6.  

Quando si usano più di una rete privata (solo cluster), controllare la configurazione del routing IPv6 per assicurarsi che il routing non sia configurato per incrociare le subnet, in quanto in questo modo si ridurranno le prestazioni di rete.  

@no__t 0Screencap della configurazione di rete automatica nell'interfaccia utente di Gestione cluster di failover @ no__t-1  
**Figura 4: Configurazione della risorsa indirizzo locale (FE80) del collegamento IPv6 automatico @ no__t-0  

## <a name="throughput-and-fault-tolerance"></a>Velocità effettiva e tolleranza di errore  
Windows Server 2019 e Windows Server 2016 rilevano automaticamente le funzionalità NIC e tenterà di usare ogni scheda di interfaccia di rete nella configurazione più veloce possibile. È possibile usare tutte le schede di rete raggruppate, NIC che usano RSS e nic con funzionalità RDMA. La tabella seguente riepiloga i compromessi quando si usano queste tecnologie. Quando si usano più schede NIC con supporto RDMA, viene garantita la massima velocità effettiva. Per ulteriori informazioni, vedere [le nozioni di base di SMB Mutlichannel](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/).

@no__t 0An illustrazione della velocità effettiva e della tolleranza di errore per varie configurazioni NIC @ no__t-1  
**Figura 5: Velocità effettiva e tolleranza di errore per varie schede NIC conifigurations @ no__t-0   

## <a name="frequently-asked-questions"></a>Domande frequenti  
**Tutte le schede di interfaccia di rete in una rete con più NIC vengono usate per il battito cardiaco del cluster?**  
    Sì.  

**Can una rete con più NIC da usare solo per le comunicazioni del cluster? Oppure può essere utilizzato solo per le comunicazioni tra client e cluster?**  
    Entrambe le configurazioni funzioneranno: tutti i ruoli di rete del cluster funzioneranno in una rete con più NIC.  

**SMB multicanale viene utilizzato anche per il traffico cluster e CSV?**  
    Sì, per impostazione predefinita tutti i cluster e il traffico CSV utilizzeranno le reti NIC disponibili. Per modificare il ruolo di rete, gli amministratori possono utilizzare i cmdlet di PowerShell per il clustering di failover o Gestione cluster di failover interfaccia utente.  

**Come è possibile visualizzare le impostazioni SMB multicanale?**  
    Usare il cmdlet **Get-SMBServerConfiguration** per cercare il valore della proprietà EnableMultiChannel.  

**La proprietà comune del cluster PlumbAllCrossSubnetRoutes è rispettata in una rete con più NIC?**  
     Sì.  

## <a name="see-also"></a>Vedere anche  
- [Novità del clustering di failover in Windows Server](whats-new-in-failover-clustering.md)  
