---
title: Clustering guest in una rete virtuale
description: Questo argomento fa parte della Guida alla rete definita dal Software su come gestire carichi di lavoro Tenant e reti virtuali in Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 5cab7e7c0ca0af848b4b58362388701cc4357860
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="guest-clustering-in-a-virtual-network"></a>Clustering guest in una rete virtuale

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Le macchine virtuali che sono connessi a una rete virtuale solo sono autorizzate a usare gli indirizzi IP assegnati Controller di rete per poter comunicare in rete.  Questo significa che le tecnologie che richiedono un indirizzo IP mobile, ad esempio Clustering di Failover di Microsoft, richiedono alcuni passaggi aggiuntivi per il corretto funzionamento di clustering.

Il metodo per rendere raggiungibili IP mobile consiste nell'usare un \(SLB\) bilanciamento del carico Software \(VIP\) IP virtuale.  Bilanciamento del carico software deve essere configurato con un probe di stato su una porta che IP in modo che SLB verrà dirigere il traffico alla macchina che attualmente ha tale IP.

## <a name="example-load-balancer-configuration"></a>Esempio: Configurazione del servizio di bilanciamento carico

Questo esempio si presuppone che hai già creato le macchine virtuali che diventeranno i nodi del cluster e li collegate a una rete virtuale.  Per istruzioni, vedere [creare una macchina virtuale e connettersi a una rete virtuale del Tenant o VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).  

In questo esempio si crea un indirizzo IP virtuale (192.168.2.100) per rappresentare l'indirizzo IP mobile del cluster e configurare un probe dello stato per monitorare la porta TCP 59999 per determinare quale nodo è attivo.

### <a name="step-1-select-the-vip"></a>Passaggio 1: Selezionare l'indirizzo VIP
Preparare assegnando un indirizzo IP di indirizzi VIP.  Tale indirizzo può essere qualsiasi indirizzo riservato o non utilizzato nella stessa subnet dei nodi del cluster.  L'indirizzo VIP deve corrispondere all'indirizzo mobile del cluster.

    $VIP = "192.168.2.100"
    $subnet = "Subnet2"
    $VirtualNetwork = "MyNetwork"
    $ResourceId = "MyNetwork_InternalVIP"

### <a name="step-2-create-the-load-balancer-properties-object"></a>Passaggio 2: Creare l'oggetto delle proprietà del servizio di bilanciamento del carico

È possibile utilizzare il comando seguente per creare l'oggetto delle proprietà del servizio di bilanciamento del carico.

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

### <a name="step-3-create-a-front-end-ip-address"></a>Passaggio 3: Creare un indirizzo IP front\-end

È possibile utilizzare il comando seguente per creare un indirizzo IP front\-end.

    $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEnd.resourceId = "Frontend1"
    $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
    $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
    $FrontEnd.properties.privateIPAddress = $VIP
    $FrontEnd.properties.privateIPAllocationMethod = "Static"

### <a name="step-4-create-a-back-end-pool-to-contain-the-cluster-nodes"></a>Passaggio 4: Creare un pool back\-end per contenere i nodi del cluster

È possibile utilizzare il comando seguente per creare un pool back\-end

    $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
    $BackEnd.resourceId = "Backend1"
    $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
    $LoadBalancerProperties.backendAddressPools += $BackEnd

### <a name="step-5-add-a-probe"></a>Passaggio 5: Aggiungere un probe
Il sensore è necessario per rilevare quali l'indirizzo mobile è attualmente attivo sul nodo del cluster.

>[!NOTE]
>La query probe indirizzo permanente della macchina virtuale sulla porta definito di seguito.  La porta deve rispondere solo nel nodo attivo. 

    $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties.protocol = "TCP"
    $lbprobe.properties.port = "59999"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11

### <a name="step-5-add-the-load-balancing-rules"></a>Passaggio 5: Aggiungere le regole di bilanciamento del carico
Questo passaggio consente di creare una regola per la porta TCP 1433 di bilanciamento del carico.  È possibile modificare il protocollo e porta in base alle esigenze.  È anche possibile ripetere questo passaggio più volte per altre porte e i protocolli su questo indirizzo VIP.  È importante che EnableFloatingIP sia impostato su $true perché in questo modo il bilanciamento del carico per inviare il pacchetto al nodo con l'indirizzo VIP originale in posizione.

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

### <a name="step-5-create-the-load-balancer-in-network-controller"></a>Passaggio 5: Creare il bilanciamento del carico in Controller di rete

È possibile utilizzare il comando seguente per creare il bilanciamento del carico.

    $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force

### <a name="step-6-add-the-cluster-nodes-to-the-backend-pool"></a>Passaggio 6: Aggiungere i nodi del cluster al pool di back-end

Questo esempio viene illustrato come aggiungere due membri del pool, ma è possibile aggiungere qualsiasi numero di nodi al pool per il cluster desiderato.

    # Cluster Node 1

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

Dopo aver creato il bilanciamento del carico e aggiungere le interfacce di rete al pool di back-end, si è pronti a configurare il cluster.  Se si utilizza un Cluster di Failover Microsoft è possibile continuare con l'esempio seguente. 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>Esempio 2: Configurazione di un Cluster di Failover di Microsoft

È possibile utilizzare la procedura seguente per configurare un cluster di failover.

### <a name="step-1-install-failover-clustering"></a>Passaggio 1: Installare il clustering di failover

È possibile utilizzare i seguenti comandi di esempio per installare e configurare le proprietà per un cluster di failover.

    add-windowsfeature failover-clustering -IncludeManagementTools
    Import-module failoverclusters

    $ClusterName = "MyCluster"
   
    $ClusterNetworkName = "Cluster Network 1"
    $IPResourceName =  
    $ILBIP = “192.168.2.100” 

    $nodes = @("DB1", "DB2")

### <a name="step-2-create-the-cluster-on-one-node"></a>Passaggio 2: Creare il cluster in un nodo

È possibile utilizzare il comando seguente per creare il cluster in un nodo.

    New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]

### <a name="step-3-stop-the-cluster-resource"></a>Passaggio 3: Interrompere la risorsa cluster

È possibile utilizzare il comando seguente per interrompere la risorsa cluster.

    Stop-ClusterResource "Cluster Name" 

### <a name="step-4-set-the-cluster-ip-and-probe-port"></a>Passaggio 4: Impostare il cluster porta IP e un probe
L'indirizzo IP deve corrispondere all'indirizzo ip front-end utilizzato nell'esempio precedente, e la porta probe deve corrispondere alla porta probe nell'esempio precedente.

    Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

### <a name="step-5-start-the-cluster-resources"></a>Passaggio 5: Avviare le risorse del cluster

È possibile utilizzare il comando seguente per avviare le risorse del cluster.

    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 

### <a name="step-6-add-the-remaining-nodes"></a>Passaggio 6: Aggiungere i nodi rimanenti

È possibile utilizzare il comando seguente per aggiungere i nodi del cluster.

    Add-ClusterNode $nodes[1]

Dopo il completamento dell'ultimo passaggio, il cluster è attivo. Traffico diretto verso l'indirizzo VIP sulla porta specificata verrà indirizzato al nodo attivo.