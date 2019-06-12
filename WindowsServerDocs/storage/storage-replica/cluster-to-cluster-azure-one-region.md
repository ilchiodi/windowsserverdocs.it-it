---
title: Cluster a cluster di Replica di archiviazione nella stessa area in Azure
description: Cluster a cluster di replica di archiviazione nella stessa area in Azure
keywords: Replica di archiviazione, gestione Server, Windows Server, Azure, Cluster, nella stessa area
author: arduppal
ms.author: arduppal
ms.date: 04/26/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 9cf998087e23f45fe5981aef6d1ff5b7b4e85b9b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447616"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>Cluster a cluster di Replica di archiviazione nella stessa area in Azure

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

È possibile configurare la replica di archiviazione da cluster a cluster nella stessa area in Azure. Negli esempi seguenti, viene usato un cluster a due nodi, ma la replica di archiviazione da cluster a cluster non è limitata a un cluster a due nodi. Nella figura seguente è un cluster a due nodi dello spazio di archiviazione diretta in grado di comunicare tra loro, sono nello stesso dominio e nella stessa area.

Guarda i video di seguito per una procedura dettagliata completa del processo.

Nella prima parte
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE26f2Y]

Seconda parte
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE269Pq]

![Il diagramma dell'architettura di Replica di archiviazione da Cluster a cluster in Azure che presenta la stessa area.](media/Cluster-to-cluster-azure-one-region/architecture.png)
> [!IMPORTANT]
> Tutti gli esempi di riferimento sono specifici dell'illustrazione precedente.

1. Creare un [gruppo di risorse](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) nel portale di Azure in un'area (**SR-AZ2AZ** nelle **Stati Uniti occidentali 2**). 
2. Creare due [set di disponibilità](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM) nel gruppo di risorse (**SR-AZ2AZ**) creato in precedenza, uno per ogni cluster. 
    a. Availability set (**az2azAS1**) b. Availability set (**az2azAS2**)
3. Creare un [rete virtuale](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-Vnet**) nel gruppo di risorse creato in precedenza (**SR-AZ2AZ**), con almeno una subnet. 
4. Creare un [gruppo di sicurezza di rete](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) e aggiungere una regola di sicurezza in ingresso per RDP:3389. È possibile scegliere di rimuovere la regola dopo aver completato l'installazione. 
5. Creazione di Windows Server [macchine virtuali](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) nel gruppo di risorse creato in precedenza (**SR-AZ2AZ**). Usare la rete virtuale creata in precedenza (**az2az-Vnet**) e il gruppo di sicurezza di rete (**az2az-NSG**). 
   
   Domain Controller (**az2azDC**). È possibile scegliere di creare un terzo set di disponibilità per il controller di dominio o aggiungere il controller di dominio in uno dei due set di disponibilità. Se si aggiunge questo al set di disponibilità creato per i due cluster, assegnare un indirizzo IP pubblico Standard durante la creazione della macchina virtuale. 
   - Installare servizi di dominio Active Directory.
   - Creare un dominio (Contoso.com)
   - Creare un utente con privilegi di amministratore (contosoadmin) 
   - Creare due macchine virtuali (**az2az1**, **az2az2**) nel set di disponibilità prima (**az2azAS1**). Assegnare un indirizzo IP pubblico standard per ogni macchina virtuale durante la creazione se stesso.
   - Aggiungere dischi gestiti di almeno 2 a ogni macchina
   - Installare la funzionalità Clustering di Failover e Replica di archiviazione
   - Creare due macchine virtuali (**az2az3**, **az2az4**) nel set di disponibilità secondo (**az2azAS2**). Assegnare l'indirizzo IP pubblico standard per ogni macchina virtuale durante la creazione se stesso. 
   - Aggiungere almeno 2 dischi gestiti per ogni macchina. 
   - Installare la funzionalità Clustering di Failover e Replica di archiviazione. 
   
6. Connettere tutti i nodi al dominio e assegnare i privilegi di amministratore per l'utente creato in precedenza. 

7. Modificare il Server DNS della rete virtuale per indirizzo IP privato del controller dominio. 
8. In questo esempio, il controller di dominio **az2azDC** dispone di indirizzo IP privato (10.3.0.8). Nella rete virtuale (**az2az-Vnet**) modificare il Server DNS 10.3.0.8. Connettere tutti i nodi per "Contoso.com" e assegnare i privilegi di amministratore per "contosoadmin".
   - Account di accesso come contosoadmin da tutti i nodi. 
    
9. La creazione dei cluster (**SRAZC1**, **SRAZC2**). 
   Ecco i comandi di PowerShell per questo esempio
   ```PowerShell
    New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```PowerShell
    New-Cluster -Name SRAZC2 -Node az2az3,az2az4 –StaticAddress 10.3.0.101
   ```
10. Abilitare spazi di archiviazione diretta
    ```PowerShell
    Enable-clusterS2D
    ```   
   
    Per ogni cluster Crea volume e il disco virtuale. Uno per i dati e un altro per il log. 
   
11. Creare uno SKU Standard interni [Load Balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) per ogni cluster (**azlbr1**,**azlbr2**). 
   
    Fornire l'indirizzo IP del Cluster come indirizzo IP privato statico per il bilanciamento del carico.
    - azlbr1 = > IP front-end: 10.3.0.100 (prelevare un indirizzo IP non usato dalla rete virtuale (**az2az-Vnet**) subnet)
    - Creare Pool back-end per ogni servizio di bilanciamento del carico. Aggiungere i nodi del cluster associati.
    - Creare Probe di integrità: porta 59999
    - Crea regola di bilanciamento carico: Consentire le porte a disponibilità elevata, con indirizzo IP mobile abilitato. 
   
    Fornire l'indirizzo IP del Cluster come indirizzo IP privato statico per il bilanciamento del carico.
    - azlbr2 = > IP front-end: 10.3.0.101 (prelevare un indirizzo IP non usato dalla rete virtuale (**az2az-Vnet**) subnet)
    - Creare Pool back-end per ogni servizio di bilanciamento del carico. Aggiungere i nodi del cluster associati.
    - Creare Probe di integrità: porta 59999
    - Crea regola di bilanciamento carico: Consentire le porte a disponibilità elevata, con indirizzo IP mobile abilitato. 
   
12. In ogni nodo del cluster, aprire la porta 59999 (Probe di integrità). 
   
    Eseguire il comando seguente in ogni nodo:
    ```PowerShell
    netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```   
13. Indicare a del cluster per l'ascolto dei messaggi di Probe di integrità su porta 59999 e rispondere dal nodo a cui appartiene questa risorsa. 
    Eseguirlo una sola volta da qualsiasi nodo del cluster, per ogni cluster. 
    
    In questo esempio, assicurarsi di modificare il "ILBIP" in base ai valori di configurazione. Eseguire il comando seguente da qualsiasi nodo **az2az1**/**az2az2**:

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}
    ```

14. Eseguire il comando seguente da qualsiasi nodo **az2az3**/**az2az4**. 

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```   
    Assicurarsi che entrambi i cluster possono connettersi / comunicano tra loro. 

    Una caratteristica "Connect to Cluster" in Gestione cluster di Failover consente di connettersi a altro cluster o controllare che altri cluster risponde da uno dei nodi del cluster corrente.  
   
    ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
    ```
    ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
    ```   

15. Creare i server di controllo cloud per entrambi i cluster. Creare due [gli account di archiviazione](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**, **az2azcw2**) in azure è per ogni cluster nello stesso gruppo di risorse (**SR-AZ2AZ**).

    - Copiare il nome account di archiviazione e la chiave da "chiavi di accesso"
    - Creare il cloud di controllo da "Gestione cluster di failover" e usare il nome dell'account e la chiave precedente per la sua creazione.

16. Eseguire [test di convalida dei cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) prima di procedere al passaggio successivo.

17. Avviare Windows PowerShell e usare il cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) per determinare se siano stati soddisfatti tutti i requisiti di Replica archiviazione. È possibile usare il cmdlet in modalità dedicata ai requisiti per un rapido test, nonché una modalità di valutazione delle prestazioni a esecuzione prolungata.

18. Configurare Replica archiviazione da cluster a cluster.
   
    Concedere l'accesso da un cluster a un altro cluster in entrambe le direzioni:

    In questo esempio:

    ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
    ```
    Se si usa Windows Server 2016, quindi eseguire anche questo comando:

    ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
    ```   
   
19. Creare SRPartnership per i cluster:</ol>

    - Per il cluster **SRAZC1**.
    - Percorso del volume:-c:\ClusterStorage\DataDisk1
    - Percorso file di registro:-g:
    - Per cluster **SRAZC2**
    - Percorso del volume:-c:\ClusterStorage\DataDisk2
    - Percorso file di registro:-g:

Eseguire il comando seguente:

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```