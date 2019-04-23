---
title: Replica di archiviazione da cluster a cluster tra aree in Azure
description: Replica di archiviazione da cluster a Cluster tra l'area di Azure
keywords: Replica di archiviazione, gestione Server, Windows Server, Azure, Cluster, tra aree, area diversa
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 41f435c3d537cbfd204dfa869d750b22200deb33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891132"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Replica di archiviazione da cluster a cluster tra aree in Azure
È possibile configurare le repliche di archiviazione da Cluster a Cluster per le applicazioni tra aree di Azure. Negli esempi seguenti, viene usato un cluster a due nodi, ma la replica di archiviazione da Cluster a Cluster non è limitata a un cluster a due nodi. Nella figura seguente è un cluster a due nodi dello spazio di archiviazione diretta che possono comunicare tra loro, si trovano nello stesso dominio e tra aree.

Guarda il video seguente per una procedura dettagliata completa del processo.
> [!video https://www.microsoft.com/en-us/videoplayer/embed/RE26xeW]

![Che presenta SR C2C in Azure all'interno del diagramma dell'architettura stessa area.](media\Cluster-to-cluster-azure-cross-region\architecture.png)
> [!IMPORTANT]
> Tutti gli esempi di riferimento sono specifici dell'illustrazione precedente.


1. Nel portale di Azure, creare [gruppi di risorse](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) in due aree diverse.

    Ad esempio, **SR-AZ2AZ** nelle **Stati Uniti occidentali 2** e **SR-AZCROSS** in **East US**, come illustrato in precedenza.

2. Creare due [set di disponibilità](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM), uno in ogni gruppo di risorse per ogni cluster
    - Availability set (**az2azAS1**) in (**SR-AZ2AZ**)
    - Set di disponibilità (**azcross-AS**) in (**SR-AZCROSS**)

3. Creare due reti virtuali
   - Creare il [rete virtuale](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-Vnet**) nel primo gruppo di risorse (**SR-AZ2AZ**), con una subnet e una subnet del Gateway.
   - Creare il [rete virtuale](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**azcross-VNET**) del secondo gruppo di risorse (**SR-AZCROSS**), con una subnet e una subnet del Gateway.

4. Creare due gruppi di sicurezza di rete
   - Creare il [gruppo di sicurezza di rete](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) nel primo gruppo di risorse (**SR-AZ2AZ**).
   - Creare il [gruppo di sicurezza di rete](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**azcross-NSG**) del secondo gruppo di risorse (**SR-AZCROSS**). 

   Aggiungere una regola di sicurezza in ingresso per RDP:3389 a due gruppi di sicurezza di rete. È possibile scegliere di rimuovere la regola dopo aver completato l'installazione.

5. Creazione di Windows Server [macchine virtuali](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) nei gruppi di risorse creato in precedenza.

   Domain Controller (**az2azDC**). È possibile scegliere di creare un set di disponibilità 3rd per il controller di dominio o aggiungere il controller di dominio in uno dei set di disponibilità di due. Se si aggiunge questo al set di disponibilità creato per i due cluster, assegnare un indirizzo IP pubblico Standard durante la creazione della macchina virtuale.
      - Installare servizi di dominio Active Directory.
      - Creare un dominio (contoso.com)
      - Creare un utente con privilegi di amministratore (contosoadmin)

   Creare due macchine virtuali (**az2az1**, **az2az2**) nel gruppo di risorse (**SR-AZ2AZ**) utilizzando la rete virtuale (**az2az-Vnet**) e gruppo di sicurezza di rete (**az2az-NSG**) nel set disponibilità (**az2azAS1**). Assegnare un indirizzo IP pubblico standard per ogni macchina virtuale durante la creazione se stesso.
      - Aggiungere almeno due dischi gestiti per ogni computer
      - Installare la funzionalità Replica archiviazione e Clustering di Failover

   Creare due macchine virtuali (**azcross1**, **azcross2**) nel gruppo di risorse (**SR-AZCROSS**) utilizzando la rete virtuale (**azcross-VNET**) e il gruppo di sicurezza di rete (**azcross-NSG**) nel set disponibilità (**azcross-AS**). Assegnare l'indirizzo IP pubblico standard per ogni macchina virtuale durante la creazione se stesso
      - Aggiungere almeno due dischi gestiti per ogni computer
      - Installare la funzionalità Replica archiviazione e Clustering di Failover

   Connettere tutti i nodi al dominio e assegnare i privilegi di amministratore per l'utente creato in precedenza.

   Modificare il Server DNS della rete virtuale per indirizzo IP privato del controller dominio.
   - Nell'esempio, il controller di dominio **az2azDC** dispone di indirizzo IP privato (10.3.0.8). Nella rete virtuale (**az2az-Vnet** e **azcross-VNET**) modificare il Server DNS 10.3.0.8. 

    Nell'esempio, la connessione di tutti i nodi per "contoso.com" e assegnare i privilegi di amministratore per "contosoadmin".
   - Account di accesso come contosoadmin da tutti i nodi. 
 
6. La creazione dei cluster (**SRAZC1**, **SRAZCross**).

   Ecco i comandi di PowerShell per l'esempio
   ```powershell
      New-Cluster -Name SRAZC1 -Node az2az1,az2az2 – StaticAddress 10.3.0.100
   ```
   ```powershell
      New-Cluster -Name SRAZCross -Node azcross1,azcross2 – StaticAddress 10.0.0.10
   ```

7. Abilitare spazi di archiviazione diretta.

   ```powershell
      Enable-clusterS2D
   ```

   > [!NOTE]
   > Per ogni cluster Crea volume e il disco virtuale. Uno per i dati e un altro per il log.

8. Creare uno SKU Standard interni [Load Balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) per ogni cluster (**azlbr1**, **azlbazcross**).

   Fornire l'indirizzo IP del Cluster come indirizzo IP privato statico per il bilanciamento del carico.
      - azlbr1 = > IP front-end: 10.3.0.100 (prelevare un indirizzo IP non usato dalla rete virtuale (**az2az-Vnet**) subnet)
      - Creare Pool back-end per ogni servizio di bilanciamento del carico. Aggiungere i nodi del cluster associati.
      - Creare Probe di integrità: porta 59999
      - Crea regola di bilanciamento carico: Consentire le porte a disponibilità elevata, con indirizzo IP mobile abilitato.

   Fornire l'indirizzo IP del Cluster come indirizzo IP privato statico per il bilanciamento del carico. 
      - azlbazcross = > Frontend IP: 10.0.0.10 (prelevare un indirizzo IP non usato dalla rete virtuale (**azcross-VNET**) subnet)
      - Creare Pool back-end per ogni servizio di bilanciamento del carico. Aggiungere i nodi del cluster associati.
      - Creare Probe di integrità: porta 59999
      - Crea regola di bilanciamento carico: Consentire le porte a disponibilità elevata, con indirizzo IP mobile abilitato. 

9. Creare [gateway di rete virtuale](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM) per la connettività Vnet-to-Vnet.

 - Creare il primo gateway di rete virtuale (**az2az VNetGateway**) nel primo gruppo di risorse (**SR-AZ2AZ**)
 - Tipo di gateway = VPN e tipo VPN = basato su Route

 - Creare il secondo gateway di rete virtuale (**azcross VNetGateway**) del secondo gruppo di risorse (**SR-AZCROSS**)
 - Tipo di gateway = VPN e tipo VPN = basato su Route

 - Creare una connessione di Vnet-to-Vnet dal primo gateway di rete virtuale al secondo gateway di rete virtuale. Fornire una chiave condivisa

 - Creare una connessione di Vnet-to-Vnet dal secondo gateway di rete virtuale al primo gateway di rete virtuale. Fornire la stessa chiave condivisa come specificato nel passaggio precedente. 

10. In ogni nodo del cluster, aprire la porta 59999 (Probe di integrità).

   Eseguire il comando seguente in ogni nodo:

   ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
   ```

11. Indicare a del cluster per l'ascolto dei messaggi di Probe di integrità su porta 59999 e rispondere dal nodo a cui appartiene questa risorsa.

   Eseguirlo una sola volta da qualsiasi nodo del cluster, per ogni cluster. 
    
   In questo esempio, assicurarsi di modificare il "ILBIP" in base ai valori di configurazione. Eseguire il comando seguente da qualsiasi nodo **az2az1**/**az2az2**

   ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
   ```

12. Eseguire il comando seguente da qualsiasi nodo **azcross1**/**azcross2**
   ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
   ```

   Assicurarsi che entrambi i cluster possono connettersi / comunicano tra loro.

   Una caratteristica "Connect to Cluster" in Gestione cluster di Failover consente di connettersi a altro cluster o controllare che altri cluster risponde da uno dei nodi del cluster corrente.

   Nell'esempio è stata usata:
   ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
   ```
   ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
   ```

13. Creare cloud di controllo per entrambi i cluster. Creare due [gli account di archiviazione](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**,**azcrosssa**) in Azure, uno per ogni cluster in ogni gruppo di risorse (**SR-AZ2AZ**,  **SR-AZCROSS**).
   
   - Copiare il nome account di archiviazione e la chiave da "chiavi di accesso"
   - Creare il cloud di controllo da "Gestione cluster di failover" e usare il nome dell'account e la chiave precedente per la sua creazione. 

14. Eseguire [test di convalida dei cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) prima di procedere al passaggio successivo

15. Avviare Windows PowerShell e usare il cmdlet [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) per determinare se siano stati soddisfatti tutti i requisiti di Replica archiviazione. È possibile usare il cmdlet in modalità dedicata ai requisiti per un rapido test, oppure in modalità di valutazione delle prestazioni a esecuzione prolungata.
 
16. Configurare la replica di archiviazione da cluster a cluster.
Concedere l'accesso da un cluster a un altro cluster in entrambe le direzioni:

   Da questo esempio:
   ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
   ```
Se si usa Windows Server 2016, quindi eseguire anche questo comando:

   ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
   ```

17. Creare SR-Partnership per i due cluster:</ol>

   - Per cluster **SRAZC1**
      - Percorso del volume:-c:\ClusterStorage\DataDisk1
      - Percorso file di registro:-g:
   - Per cluster **SRAZCross**
      - Percorso del volume:-c:\ClusterStorage\DataDiskCross
      - Percorso file di registro:-g:

Eseguire il comando:

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```