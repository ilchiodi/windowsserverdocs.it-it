---
title: Replica di archiviazione da cluster a cluster tra aree in Azure
description: Replica di archiviazione da cluster a cluster tra aree in Azure
keywords: Replica di archiviazione, Server Manager, Windows Server, Azure, cluster, area geografica, area diversa
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 806857d5de067c0f4640344ed80338b474dd758e
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950067"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Replica di archiviazione da cluster a cluster tra aree in Azure

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (Canale semestrale)

È possibile configurare il cluster per le repliche di archiviazione del cluster per le applicazioni tra aree in Azure. Negli esempi seguenti viene usato un cluster a due nodi, ma la replica di archiviazione da cluster a cluster non è limitata a un cluster a due nodi. L'illustrazione seguente è un cluster con spazio di archiviazione diretto a due nodi che può comunicare tra loro, si trovano nello stesso dominio e sono tra più aree.

Guardare il video seguente per una procedura dettagliata completa del processo.
> [!video https://www.microsoft.com/videoplayer/embed/RE26xeW]

![Il diagramma dell'architettura che mostra C2C SR in Azure con la stessa area.](media/Cluster-to-cluster-azure-cross-region/architecture.png)
> [!IMPORTANT]
> Tutti gli esempi a cui si fa riferimento sono specifici dell'illustrazione precedente.


1. Nella portale di Azure creare gruppi di [risorse](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) in due aree diverse.

    Ad esempio, **SR-AZ2AZ** negli Stati **Uniti occidentali 2** e **SR-AZCROSS** negli **Stati Uniti centro-occidentali**, come illustrato in precedenza.

2. Creare due [set di disponibilità](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM), uno in ogni gruppo di risorse per ogni cluster.
    - Set di disponibilità (**az2azAS1**) in (**SR-AZ2AZ**)
    - Set di disponibilità (**azcross-As**) in (**SR-azcross**)

3. Creare due reti virtuali
   - Creare la [rete virtuale](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-VNET**) nel primo gruppo di risorse (**SR-az2az**), con una subnet e una subnet del gateway.
   - Creare la [rete virtuale](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**azcross-VNET**) nel secondo gruppo di risorse (**SR-azcross**), con una subnet e una subnet del gateway.

4. Creare due gruppi di sicurezza di rete
   - Creare il [gruppo di sicurezza di rete](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) nel primo gruppo di risorse (**SR-az2az**).
   - Creare il [gruppo di sicurezza di rete](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**azcross-NSG**) nel secondo gruppo di risorse (**SR-azcross**).

   Aggiungere una regola di sicurezza in ingresso per RDP: 3389 a entrambi i gruppi di sicurezza di rete. È possibile scegliere di rimuovere questa regola dopo aver completato l'installazione.

5. Creare [macchine virtuali](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) Windows Server nei gruppi di risorse creati in precedenza.

   Controller di dominio (**az2azDC**). È possibile scegliere di creare un terzo set di disponibilità per il controller di dominio o di aggiungere il controller di dominio in uno dei due set di disponibilità. Se si aggiunge questo valore al set di disponibilità creato per i due cluster, assegnargli un indirizzo IP pubblico standard durante la creazione della macchina virtuale.
      - Installare Dominio di Active Directory servizio.
      - Creazione di un dominio (contoso.com)
      - Creazione di un utente con privilegi di amministratore (ContosoAdmin)

   Creare due macchine virtuali (**az2az1**, **az2az2**) nel gruppo di risorse (**SR-AZ2AZ**) usando la rete virtuale (**AZ2AZ-VNET**) e il gruppo di sicurezza di rete (AZ2AZ **-NSG**) nel set di disponibilità (**az2azAS1**). Assegnare un indirizzo IP pubblico standard a ogni macchina virtuale durante la creazione.
      - Aggiungere almeno due dischi gestiti a ogni computer
      - Installare il clustering di failover e la funzionalità di replica di archiviazione

   Creare due macchine virtuali (**azcross1**, **azcross2**) nel gruppo di risorse (**SR-AZCROSS**) usando la rete virtuale (**AZCROSS-VNET**) e il gruppo di sicurezza di rete (**AZCROSS-NSG**) nel set di disponibilità (**AZCROSS-As**). Assegnare un indirizzo IP pubblico standard a ogni macchina virtuale durante la creazione
      - Aggiungere almeno due dischi gestiti a ogni computer
      - Installare il clustering di failover e la funzionalità di replica di archiviazione

   Connettere tutti i nodi al dominio e fornire privilegi di amministratore all'utente creato in precedenza.

   Modificare il server DNS della rete virtuale in un indirizzo IP privato del controller di dominio.
   - Nell'esempio, il controller di dominio **az2azDC** ha un indirizzo IP privato (10.3.0.8). Nella rete virtuale (**az2az-VNET** e **azcross-VNET**) modificare il server DNS 10.3.0.8. 

     Nell'esempio, connettere tutti i nodi a "contoso.com" e specificare i privilegi di amministratore per "ContosoAdmin".
   - Accedere come ContosoAdmin da tutti i nodi. 
 
6. Creare i cluster (**SRAZC1**, **SRAZCross**).

   Di seguito sono riportati i comandi di PowerShell per l'esempio
   ```powershell
      New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```powershell
      New-Cluster -Name SRAZCross -Node azcross1,azcross2 –StaticAddress 10.0.0.10
   ```

7. Abilitare spazi di archiviazione diretta.

   ```powershell
      Enable-clusterS2D
   ```

   > [!NOTE]
   > Per ogni cluster creare un disco virtuale e un volume. Uno per i dati e un altro per il log.

8. Creare uno SKU standard interno [Load Balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) per ogni cluster (**azlbr1**, **azlbazcross**).

   Fornire l'indirizzo IP del cluster come indirizzo IP privato statico per il servizio di bilanciamento del carico.
      - azlbr1 = > IP front-end: 10.3.0.100 (preleva un indirizzo IP non usato dalla subnet della rete virtuale (**az2az-VNET**))
      - Creare il pool back-end per ogni servizio di bilanciamento del carico. Aggiungere i nodi del cluster associati.
      - Creare un probe di integrità: porta 59999
      - Creare una regola di bilanciamento del carico: consentire le porte a disponibilità elevata, con IP mobile abilitato.

   Fornire l'indirizzo IP del cluster come indirizzo IP privato statico per il servizio di bilanciamento del carico. 
      - azlbazcross = > IP front-end: 10.0.0.10 (preleva un indirizzo IP non usato dalla subnet della rete virtuale (**azcross-VNET**))
      - Creare il pool back-end per ogni servizio di bilanciamento del carico. Aggiungere i nodi del cluster associati.
      - Creare un probe di integrità: porta 59999
      - Creare una regola di bilanciamento del carico: consentire le porte a disponibilità elevata, con IP mobile abilitato. 

9. Creare un [gateway di rete virtuale](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM) per la connettività da VNET a vnet.

   - Creare il primo gateway di rete virtuale (**az2az-VNetGateway**) nel primo gruppo di risorse (**SR-az2az**)
   - Tipo di gateway = VPN e tipo VPN = basato su Route

   - Creare il secondo gateway di rete virtuale (**azcross-VNetGateway**) nel secondo gruppo di risorse (**SR-azcross**)
   - Tipo di gateway = VPN e tipo VPN = basato su Route

   - Creare una connessione da VNET a VNET dal primo gateway di rete virtuale al secondo gateway di rete virtuale. Fornire una chiave condivisa

   - Creare una connessione da VNET a VNET dal secondo gateway di rete virtuale al primo gateway di rete virtuale. Fornire la stessa chiave condivisa come indicato nel passaggio precedente. 

10. In ogni nodo del cluster aprire la porta 59999 (Probe di integrità).

    Eseguire il comando seguente in ogni nodo:

    ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```

11. Indica al cluster di restare in ascolto dei messaggi di probe di integrità sulla porta 59999 e rispondere dal nodo che attualmente possiede questa risorsa.

    Eseguirlo una sola volta da un nodo qualsiasi del cluster, per ogni cluster. 
    
    In questo esempio, assicurarsi di modificare "ILBIP" in base ai valori di configurazione. Eseguire il comando seguente da un nodo **az2az1**/**az2az2**

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

12. Eseguire il comando seguente da un nodo **azcross1**/**azcross2**
    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

    Assicurarsi che entrambi i cluster possano connettersi/comunicare tra loro.

    Utilizzare la funzionalità "Connetti a cluster" in Gestione cluster di failover per connettersi all'altro cluster o controllare che altri cluster rispondano da uno dei nodi del cluster corrente.

    Dall'esempio usato:
    ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
    ```
    ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
    ```

13. Creare il server di controllo del cloud per entrambi i cluster. Creare due [account di archiviazione](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**,**azcrosssa**) in Azure, uno per ogni cluster in ogni gruppo di risorse (**SR-AZ2AZ**, **SR-AZCROSS**).
   
    - Copiare il nome e la chiave dell'account di archiviazione da "chiavi di accesso"
    - Creare il cloud di controllo da "Gestione cluster di failover" e usare il nome e la chiave dell'account precedente per crearlo. 

14. Eseguire i [test di convalida del cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) prima di passare al passaggio successivo

15. Avviare Windows PowerShell e usare il cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) per determinare se siano stati soddisfatti tutti i requisiti di Replica archiviazione. È possibile usare il cmdlet in modalità dedicata ai requisiti per un rapido test, oppure in modalità di valutazione delle prestazioni a esecuzione prolungata.
 
16. Configurare la replica di archiviazione da cluster a cluster.
    Concedere l'accesso da un cluster a un altro cluster in entrambe le direzioni:

    Dall'esempio:
    ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
    ```
    Se usi Windows Server 2016, Esegui anche questo comando:

    ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
    ```

17. Creare SR-partnership per i due cluster:</ol>

    - Per **SRAZC1** cluster
      - Percorso volume:-c:\ClusterStorage\DataDisk1
      - Percorso log:-g:
    - Per **SRAZCross** cluster
      - Percorso volume:-c:\ClusterStorage\DataDiskCross
      - Percorso log:-g:

Eseguire il comando seguente:

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```