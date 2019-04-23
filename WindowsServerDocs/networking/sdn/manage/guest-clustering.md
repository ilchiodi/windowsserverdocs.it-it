---
title: Clustering guest in una rete virtuale
description: Le macchine virtuali connesse a una rete virtuale possono solo usare gli indirizzi IP che il Controller di rete è assegnato per comunicare sulla rete.  Le tecnologie di clustering che richiedono un indirizzo IP mobile, ad esempio Clustering di Failover Microsoft richiedono alcuni passaggi aggiuntivi per funzionare correttamente.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: shortpatti
ms.date: 08/26/2018
ms.openlocfilehash: fcd37ebb3739f1d7118ce41dfc61764486c920d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844962"
---
# <a name="guest-clustering-in-a-virtual-network"></a>Clustering guest in una rete virtuale

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Le macchine virtuali connesse a una rete virtuale possono solo usare gli indirizzi IP che il Controller di rete è assegnato per comunicare sulla rete.  Le tecnologie di clustering che richiedono un indirizzo IP mobile, ad esempio Clustering di Failover Microsoft richiedono alcuni passaggi aggiuntivi per funzionare correttamente.

Il metodo per rendere raggiungibili l'indirizzo IP mobile consiste nell'usare un servizio di bilanciamento del carico Software \(SLB\) IP virtuale \(VIP\).  Il servizio di bilanciamento del carico software deve essere configurato con un probe di integrità su una porta su tale indirizzo IP in modo da bilanciamento del carico software indirizza il traffico alla macchina che attualmente dispone di tale indirizzo IP.


## <a name="example-load-balancer-configuration"></a>Esempio: Configurazione del bilanciamento del carico

Questo esempio si presuppone di avere già creato le macchine virtuali che diventeranno i nodi del cluster e li collegata a una rete virtuale.  Per indicazioni, vedere [creare una macchina virtuale e connettersi a una rete virtuale del Tenant o VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).  

In questo esempio si crea un indirizzo IP virtuale (192.168.2.100) per rappresentare l'indirizzo IP mobile del cluster e configurare un probe di integrità per monitorare la porta TCP 59999 per determinare quale nodo è quello attivo.

1. Selezionare l'indirizzo VIP.<p>Preparare assegnando un indirizzo IP VIP, che può essere qualsiasi indirizzo riservato o inutilizzato nella stessa subnet dei nodi del cluster.  L'indirizzo VIP deve corrispondere all'indirizzo mobile del cluster.

   ```PowerShell
   $VIP = "192.168.2.100"
   $subnet = "Subnet2"
   $VirtualNetwork = "MyNetwork"
   $ResourceId = "MyNetwork_InternalVIP"
   ```

2. Creare l'oggetto di proprietà di bilanciamento del carico.

   ```PowerShell
   $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

3. Creare un front\-indirizzo IP finale.

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

4. Creare una copia di\-end pool contenga nodi del cluster.

   ```PowerShell
   $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
   $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
   $BackEnd.resourceId = "Backend1"
   $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
   $LoadBalancerProperties.backendAddressPools += $BackEnd
   ```

5. Aggiungere un probe per rilevare l'indirizzo a virgola mobile è attualmente attivo in quale nodo del cluster. 

   >[!NOTE]
   >La query di tipo probe sul indirizzo permanente della VM sulla porta definita di seguito.  La porta deve rispondere solo nel nodo attivo. 

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

6. Aggiungere le regole per la porta TCP 1433 di bilanciamento del carico.<p>È possibile modificare il protocollo e porte in base alle esigenze.  È inoltre possibile ripetere questo passaggio più volte per porte aggiuntive e protcols su questo indirizzo VIP.  È importante che EnableFloatingIP è impostato su $true perché indica il bilanciamento del carico per inviare il pacchetto per il nodo con un indirizzo VIP nella posizione originale.

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

7. Creare il servizio di bilanciamento del carico in Controller di rete.

   ```PowerShell
   $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force
   ```

8. Aggiungere i nodi del cluster per il pool back-end.<p>È possibile aggiungere tutti i nodi nel pool è necessario per il cluster.

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

   Dopo aver creato il servizio di bilanciamento del carico e aggiunte le interfacce di rete al pool back-end, si è pronti per configurare il cluster.  

9. (Facoltativo) Se si usa un Cluster di Failover Microsoft, continuare con l'esempio successivo. 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>Esempio 2: Configurazione di un cluster di failover di Microsoft

È possibile utilizzare la procedura seguente per configurare un cluster di failover.

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

4. Impostare il cluster porta IP e un probe.<p>L'indirizzo IP deve corrispondere all'indirizzo ip front-end usato nell'esempio precedente, e la porta probe deve corrispondere alla porta di probe dell'esempio precedente.

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

_**Il cluster è attivo.**_ Viene indirizzato il traffico verso l'indirizzo VIP sulla porta specificata in corrispondenza del nodo attivo.

---