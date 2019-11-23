---
title: Clustering guest in una rete virtuale
description: Le macchine virtuali connesse a una rete virtuale possono usare solo gli indirizzi IP assegnati dal controller di rete per comunicare sulla rete.  Per il corretto funzionamento delle tecnologie di clustering che richiedono un indirizzo IP mobile, ad esempio il clustering di failover Microsoft, sono necessari alcuni passaggi aggiuntivi.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: shortpatti
ms.date: 08/26/2018
ms.openlocfilehash: 05704beeae27bd9de9ad0c5cf578581c650a976f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406036"
---
# <a name="guest-clustering-in-a-virtual-network"></a>Clustering guest in una rete virtuale

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Le macchine virtuali connesse a una rete virtuale possono usare solo gli indirizzi IP assegnati dal controller di rete per comunicare sulla rete.  Per il corretto funzionamento delle tecnologie di clustering che richiedono un indirizzo IP mobile, ad esempio il clustering di failover Microsoft, sono necessari alcuni passaggi aggiuntivi.

Il metodo per rendere la portabile IP mobile raggiungibile consiste nell'usare un software Load Balancer \(SLB\) indirizzo IP virtuale \(VIP\).  Il servizio di bilanciamento del carico software deve essere configurato con un probe di integrità su una porta sull'IP in modo che SLB indirizza il traffico al computer che attualmente ha tale indirizzo IP.


## <a name="example-load-balancer-configuration"></a>Esempio: configurazione del servizio di bilanciamento del carico

Questo esempio presuppone che siano già state create le VM che diventeranno nodi del cluster e che siano collegate a una rete virtuale.  Per informazioni aggiuntive, vedere [creare una macchina virtuale e connettersi a una rete virtuale del tenant o a una VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).  

In questo esempio viene creato un indirizzo IP virtuale (192.168.2.100) per rappresentare l'indirizzo IP mobile del cluster e viene configurato un probe di integrità per monitorare la porta TCP 59999 per determinare quale nodo è quello attivo.

1. Selezionare l'indirizzo VIP.<p>Preparare assegnando un indirizzo IP VIP, che può essere qualsiasi indirizzo inutilizzato o riservato nella stessa subnet dei nodi del cluster.  Il VIP deve corrispondere all'indirizzo mobile del cluster.

   ```PowerShell
   $VIP = "192.168.2.100"
   $subnet = "Subnet2"
   $VirtualNetwork = "MyNetwork"
   $ResourceId = "MyNetwork_InternalVIP"
   ```

2. Creare l'oggetto proprietà del servizio di bilanciamento del carico.

   ```PowerShell
   $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

3. Creare un indirizzo IP front\-end.

   ```PowerShell
   $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
   $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
   $FrontEnd.resourceId = "Frontend1"
   $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
   $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
   $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
   $FrontEnd.properties.privateIPAddress = $VIP
   $FrontEnd.properties.privateIPAllocationMethod = "Static"
   ```

4. Creare un pool back\-end per contenere i nodi del cluster.

   ```PowerShell
   $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
   $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
   $BackEnd.resourceId = "Backend1"
   $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
   $LoadBalancerProperties.backendAddressPools += $BackEnd
   ```

5. Aggiungere un probe per individuare il nodo del cluster in cui l'indirizzo mobile è attualmente attivo. 

   >[!NOTE]
   >La query Probe sull'indirizzo permanente della macchina virtuale nella porta definita di seguito.  La porta deve rispondere solo sul nodo attivo. 

   ```PowerShell
   $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
   $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

   $lbprobe.ResourceId = "Probe1"
   $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
   $lbprobe.properties.protocol = "TCP"
   $lbprobe.properties.port = "59999"
   $lbprobe.properties.IntervalInSeconds = 5
   $lbprobe.properties.NumberOfProbes = 11
   ```

6. Aggiungere le regole di bilanciamento del carico per la porta TCP 1433.<p>È possibile modificare il protocollo e la porta in base alle esigenze.  È anche possibile ripetere questo passaggio più volte per le porte aggiuntive e protcols in questo indirizzo VIP.  È importante che abilitazione sia impostato su $true perché questo indica al servizio di bilanciamento del carico di inviare il pacchetto al nodo con il VIP originale sul posto.

   ```PowerShell
   $LoadBalancerProperties.loadbalancingRules += $lbrule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
   $lbrule.properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
   $lbrule.ResourceId = "Rules1"

   $lbrule.properties.frontendipconfigurations += $FrontEnd
   $lbrule.properties.backendaddresspool = $BackEnd 
   $lbrule.properties.protocol = "TCP"
   $lbrule.properties.frontendPort = $lbrule.properties.backendPort = 1433 
   $lbrule.properties.IdleTimeoutInMinutes = 4
   $lbrule.properties.EnableFloatingIP = $true
   $lbrule.properties.Probe = $lbprobe
   ```

7. Creare il servizio di bilanciamento del carico nel controller di rete.

   ```PowerShell
   $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force
   ```

8. Aggiungere i nodi del cluster al pool back-end.<p>È possibile aggiungere al pool tutti i nodi necessari per il cluster.

   ```PowerShell
   # Cluster Node 1

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force
   ```

   Dopo aver creato il servizio di bilanciamento del carico e aggiunto le interfacce di rete al pool back-end, si è pronti per configurare il cluster.  

9. Opzionale Se si usa un cluster di failover Microsoft, continuare con l'esempio successivo. 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>Esempio 2: configurazione di un cluster di failover Microsoft

Per configurare un cluster di failover, è possibile utilizzare i passaggi seguenti.

1. Installare e configurare le proprietà per un cluster di failover.

   ```PowerShell
   add-windowsfeature failover-clustering -IncludeManagementTools
   Import-module failoverclusters

   $ClusterName = "MyCluster"
   
   $ClusterNetworkName = "Cluster Network 1"
   $IPResourceName =  
   $ILBIP = “192.168.2.100” 

   $nodes = @("DB1", "DB2")
   ```

2. Creare il cluster in un nodo.

   ```PowerShell
   New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]
   ```

3. Arrestare la risorsa cluster.

   ```PowerShell
   Stop-ClusterResource "Cluster Name" 
   ```

4. Impostare l'IP del cluster e la porta Probe.<p>L'indirizzo IP deve corrispondere all'indirizzo IP front-end usato nell'esempio precedente e la porta Probe deve corrispondere alla porta Probe nell'esempio precedente.

   ```PowerShell
   Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

5. Avviare le risorse del cluster.

   ```PowerShell
    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 
   ```

6. Aggiungere i nodi rimanenti.

   ```PowerShell
   Add-ClusterNode $nodes[1]
   ```

_**Il cluster è attivo.**_ Il traffico verso l'indirizzo VIP sulla porta specificata viene indirizzato al nodo attivo.

---