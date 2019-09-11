---
title: Replica di archiviazione da cluster a cluster nella stessa area di Azure
description: Replica di archiviazione da cluster a cluster nella stessa area di Azure
keywords: Replica di archiviazione, Server Manager, Windows Server, Azure, cluster, la stessa area
author: arduppal
ms.author: arduppal
ms.date: 04/26/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 7e9418e398175a6b1caf63a36593f36d66a7c943
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869793"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>Replica di archiviazione da cluster a cluster nella stessa area di Azure

> Si applica a Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)

È possibile configurare il cluster per la replica di archiviazione del cluster nella stessa area di Azure. Negli esempi seguenti viene usato un cluster a due nodi, ma la replica di archiviazione da cluster a cluster non è limitata a un cluster a due nodi. L'illustrazione seguente è un cluster con spazio di archiviazione diretto a due nodi che può comunicare tra loro, si trovano nello stesso dominio e all'interno della stessa area.

Guarda i video seguenti per una procedura dettagliata completa del processo.

Parte 1
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE26f2Y]

Seconda parte
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE269Pq]

![Il diagramma dell'architettura che mostra la replica di archiviazione da cluster a cluster in Azure nella stessa area.](media/Cluster-to-cluster-azure-one-region/architecture.png)
> [!IMPORTANT]
> Tutti gli esempi a cui si fa riferimento sono specifici dell'illustrazione precedente.

1. Creare un [gruppo di risorse](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) nel portale di Azure in un'area (**SR-AZ2AZ** negli **Stati Uniti occidentali 2**). 
2. Creare due [set di disponibilità](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM) nel gruppo di risorse (**SR-AZ2AZ**) creato in precedenza, uno per ogni cluster. 
    a. Set di disponibilità (**az2azAS1**) b. Set di disponibilità (**az2azAS2**)
3. Creare una [rete virtuale](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-VNET**) nel gruppo di risorse creato in precedenza (**SR-az2az**), con almeno una subnet. 
4. Creare un [gruppo di sicurezza di rete](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) e aggiungere una regola di sicurezza in ingresso per RDP: 3389. È possibile scegliere di rimuovere questa regola dopo aver completato l'installazione. 
5. Creare [macchine virtuali](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) Windows Server nel gruppo di risorse creato in precedenza (**SR-AZ2AZ**). Usare la rete virtuale creata in precedenza (**az2az-VNET**) e il gruppo di sicurezza di rete (**az2az-NSG**). 
   
   Controller di dominio (**az2azDC**). È possibile scegliere di creare un terzo set di disponibilità per il controller di dominio o di aggiungere il controller di dominio in uno dei due set di disponibilità. Se si aggiunge questo valore al set di disponibilità creato per i due cluster, assegnargli un indirizzo IP pubblico standard durante la creazione della macchina virtuale. 
   - Installare Dominio di Active Directory servizio.
   - Creazione di un dominio (Contoso.com)
   - Creazione di un utente con privilegi di amministratore (ContosoAdmin) 
   - Creare due macchine virtuali (**az2az1**, **az2az2**) nel primo set di disponibilità (**az2azAS1**). Assegnare un indirizzo IP pubblico standard a ogni macchina virtuale durante la creazione.
   - Aggiungere almeno 2 dischi gestiti a ogni computer
   - Installare la funzionalità di clustering di failover e replica di archiviazione
   - Creare due macchine virtuali (**az2az3**, **az2az4**) nel secondo set di disponibilità (**az2azAS2**). Assegnare un indirizzo IP pubblico standard a ogni macchina virtuale durante la creazione. 
   - Aggiungere almeno 2 dischi gestiti a ogni computer. 
   - Installare la funzionalità clustering di failover e replica di archiviazione. 
   
6. Connettere tutti i nodi al dominio e fornire privilegi di amministratore all'utente creato in precedenza. 

7. Modificare il server DNS della rete virtuale in un indirizzo IP privato del controller di dominio. 
8. In questo esempio, il controller di dominio **az2azDC** ha un indirizzo IP privato (10.3.0.8). Nella rete virtuale (**az2az-VNET**) modificare il server DNS 10.3.0.8. Connettere tutti i nodi a "Contoso.com" e specificare i privilegi di amministratore per "ContosoAdmin".
   - Accedere come ContosoAdmin da tutti i nodi. 
    
9. Creare i cluster (**SRAZC1**, **SRAZC2**). 
   Di seguito sono riportati i comandi di PowerShell per l'esempio
   ```PowerShell
    New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```PowerShell
    New-Cluster -Name SRAZC2 -Node az2az3,az2az4 –StaticAddress 10.3.0.101
   ```
10. Abilita spazi di archiviazione diretta
    ```PowerShell
    Enable-clusterS2D
    ```   
   
    Per ogni cluster creare un disco virtuale e un volume. Uno per i dati e un altro per il log. 
   
11. Creare uno SKU standard interno [Load Balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) per ogni cluster (**azlbr1**,**azlbr2**). 
   
    Fornire l'indirizzo IP del cluster come indirizzo IP privato statico per il servizio di bilanciamento del carico.
    - azlbr1 = > IP front-end: 10.3.0.100 (preleva un indirizzo IP non usato dalla subnet della rete virtuale (**az2az-VNET**))
    - Creare il pool back-end per ogni servizio di bilanciamento del carico. Aggiungere i nodi del cluster associati.
    - Creare un probe di integrità: porta 59999
    - Creare una regola di bilanciamento del carico: Consenti le porte a disponibilità elevata, con IP mobile abilitato. 
   
    Fornire l'indirizzo IP del cluster come indirizzo IP privato statico per il servizio di bilanciamento del carico.
    - azlbr2 = > IP front-end: 10.3.0.101 (preleva un indirizzo IP non usato dalla subnet della rete virtuale (**az2az-VNET**))
    - Creare il pool back-end per ogni servizio di bilanciamento del carico. Aggiungere i nodi del cluster associati.
    - Creare un probe di integrità: porta 59999
    - Creare una regola di bilanciamento del carico: Consenti le porte a disponibilità elevata, con IP mobile abilitato. 
   
12. In ogni nodo del cluster aprire la porta 59999 (Probe di integrità). 
   
    Eseguire il comando seguente in ogni nodo:
    ```PowerShell
    netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```   
13. Indica al cluster di restare in ascolto dei messaggi di probe di integrità sulla porta 59999 e rispondere dal nodo che attualmente possiede questa risorsa. 
    Eseguirlo una sola volta da un nodo qualsiasi del cluster, per ogni cluster. 
    
    In questo esempio, assicurarsi di modificare "ILBIP" in base ai valori di configurazione. Eseguire il comando seguente da un nodo **az2az1**/**az2az2**:

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}
    ```

14. Eseguire il comando seguente da un nodo **az2az3**/**az2az4**. 

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```   
    Assicurarsi che entrambi i cluster possano connettersi/comunicare tra loro. 

    Utilizzare la funzionalità "Connetti a cluster" in Gestione cluster di failover per connettersi all'altro cluster o controllare che altri cluster rispondano da uno dei nodi del cluster corrente.  
   
    ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
    ```
    ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
    ```   

15. Creare i testimoni cloud per entrambi i cluster. Creare due [account di archiviazione](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**, **az2azcw2**) in Azure uno per ogni cluster nello stesso gruppo di risorse (**SR-AZ2AZ**).

    - Copiare il nome e la chiave dell'account di archiviazione da "chiavi di accesso"
    - Creare il cloud di controllo da "Gestione cluster di failover" e usare il nome e la chiave dell'account precedente per crearlo.

16. Eseguire i [test di convalida del cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) prima di passare al passaggio successivo.

17. Avviare Windows PowerShell e usare il cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) per determinare se siano stati soddisfatti tutti i requisiti di Replica archiviazione. È possibile utilizzare il cmdlet in modalità solo requisiti per un test rapido, oltre a una modalità di valutazione delle prestazioni a esecuzione prolungata.

18. Configurare la replica di archiviazione da cluster a cluster.
   
    Concedere l'accesso da un cluster a un altro cluster in entrambe le direzioni:

    Nell'esempio:

    ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
    ```
    Se si usa Windows Server 2016, eseguire anche questo comando:

    ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
    ```   
   
19. Creare SRPartnership per i cluster:</ol>

    - Per il cluster **SRAZC1**.
    - Percorso volume:-c:\ClusterStorage\DataDisk1
    - Percorso log:-g:
    - Per **SRAZC2** cluster
    - Percorso volume:-c:\ClusterStorage\DataDisk2
    - Percorso log:-g:

Eseguire il comando seguente:

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```