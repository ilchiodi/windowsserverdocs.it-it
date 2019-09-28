---
title: Configurare il servizio di bilanciamento del carico software per il bilanciamento del carico e Network Address Translation (NAT)
description: Questo argomento fa parte della Guida Software Defined Networking su come gestire i carichi di lavoro e le reti virtuali dei tenant in Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 80f1319c1abc845d7e63a2d53868bf7a3c381019
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406093"
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>Configurare il servizio di bilanciamento del carico software per il bilanciamento del carico e Network Address Translation (NAT)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni su come usare Software Defined Networking \(Sdn\) software Load Balancer \(SLB\) per fornire Network Address Translation \(NAT\)in uscita, NAT in ingresso o bilanciamento del carico tra più istanze di un'applicazione.

## <a name="software-load-balancer-overview"></a>Panoramica di Load Balancer software

Il software Sdn Load Balancer \(SLB\) offre disponibilità elevata e prestazioni di rete per le applicazioni. È un servizio di bilanciamento \(del carico UDP\) di livello 4 che distribuisce il traffico in ingresso tra istanze del servizio integre in servizi cloud o macchine virtuali definite in un set di bilanciamento del carico.

Configurare SLB per eseguire le operazioni seguenti:

- Bilanciare il carico del traffico in ingresso esterno a una rete virtuale \(alle\)VM di macchine virtuali, detto anche bilanciamento del carico VIP pubblico.
- Bilanciare il carico del traffico in ingresso tra le macchine virtuali in una rete virtuale, tra macchine virtuali in servizi cloud o tra computer locali e macchine virtuali in una rete virtuale cross-premise. 
- Inviare il traffico di rete della macchina virtuale dalla rete virtuale a destinazioni esterne usando Network Address Translation (NAT), detto anche NAT in uscita.
- Inviare il traffico esterno a una VM specifica, chiamata anche NAT in ingresso.




## <a name="example-create-a-public-vip-for-load-balancing-a-pool-of-two-vms-on-a-virtual-network"></a>Esempio: Creare un indirizzo VIP pubblico per il bilanciamento del carico di un pool di due macchine virtuali in una rete virtuale

In questo esempio viene creato un oggetto di bilanciamento del carico con un indirizzo VIP pubblico e due macchine virtuali come membri del pool per soddisfare le richieste all'indirizzo VIP. Questo codice di esempio aggiunge anche un probe di integrità HTTP per rilevare se uno dei membri del pool non risponde.

1. Preparare l'oggetto del servizio di bilanciamento del carico.

   ```PowerShell
    import-module NetworkController

    $URI = "https://sdn.contoso.com"

    $LBResourceId = "LB2"

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

2. Assegnare un indirizzo IP front-end, comunemente definito IP virtuale (VIP).<p>Il VIP deve provenire da un indirizzo IP non usato in uno dei pool IP di rete logica assegnati al gestore del servizio di bilanciamento del carico. 

   ```PowerShell
    $VIPIP = "10.127.134.5"
    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException
    
    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"
      
    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig
   ```

3. Allocare un pool di indirizzi back-end che contiene gli indirizzi IP dinamici (DIP) che costituiscono i membri del set con carico bilanciato di macchine virtuali. 

   ```PowerShell 
    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"

    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

4. Definire un probe di integrità usato dal servizio di bilanciamento del carico per determinare lo stato di integrità dei membri del pool back-end.<p>In questo esempio viene definito un probe HTTP che esegue una query su RequestPath di "/Health.htm."  La query viene eseguita ogni 5 secondi, come specificato dalla proprietà IntervalInSeconds.<p>Il probe di integrità deve ricevere un codice di risposta HTTP 200 per 11 query consecutive affinché il probe consideri l'IP back-end in modo che sia integro. Se l'indirizzo IP back-end non è integro, non riceve il traffico dal servizio di bilanciamento del carico.

   >[!IMPORTANT]
   >Non bloccare il traffico verso o dal primo IP nella subnet per gli elenchi di controllo di accesso (ACL) applicati all'IP back-end, perché si tratta del punto di origine per i probe.

   Usare l'esempio seguente per definire un probe di integrità.

   ```PowerShell
    $Probe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $Probe.ResourceId = "Probe1"
    $Probe.ResourceRef = "/loadBalancers/$LBResourceId/Probes/$($Probe.ResourceId)"

    $Probe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties
    $Probe.properties.Protocol = "HTTP"
    $Probe.properties.Port = "80"
    $Probe.properties.RequestPath = "/health.htm"
    $Probe.properties.IntervalInSeconds = 5
    $Probe.properties.NumberOfProbes = 11

    $LoadBalancerProperties.Probes += $Probe
   ```

5. Definire una regola di bilanciamento del carico per inviare il traffico che arriva all'indirizzo IP front-end all'indirizzo IP back-end.  In questo esempio, il pool back-end riceve il traffico TCP sulla porta 80.<p>Usare l'esempio seguente per definire una regola di bilanciamento del carico:

   ```PowerShell
   $Rule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
   $Rule.ResourceId = "webserver1"

   $Rule.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
   $Rule.Properties.FrontEndIPConfigurations += $FrontEndIPConfig
   $Rule.Properties.backendaddresspool = $BackEndAddressPool 
   $Rule.Properties.protocol = "TCP"
   $Rule.Properties.FrontEndPort = 80
   $Rule.Properties.BackEndPort = 80
   $Rule.Properties.IdleTimeoutInMinutes = 4
   $Rule.Properties.Probe = $Probe

   $LoadBalancerProperties.loadbalancingRules += $Rule
   ```

6. Aggiungere la configurazione del servizio di bilanciamento del carico al controller di rete.<p>Usare l'esempio seguente per aggiungere la configurazione del servizio di bilanciamento del carico al controller di rete:

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

7. Seguire l'esempio seguente per aggiungere le interfacce di rete a questo pool back-end.


## <a name="example-use-slb-for-outbound-nat"></a>Esempio: Usare SLB per NAT in uscita

In questo esempio viene configurato SLB con un pool back-end per fornire la funzionalità NAT in uscita per una VM nello spazio degli indirizzi privato di una rete virtuale per raggiungere il traffico in uscita verso Internet. 

1. Creare le proprietà del servizio di bilanciamento del carico, l'indirizzo IP front-end e il pool back-end.

   ```PowerShell
    import-module NetworkController
    $URI = "https://sdn.contoso.com"

    $LBResourceId = "OutboundNATMMembers"
    $VIPIP = "10.127.134.6"

    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"

    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig

    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"
    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

2. Definire la regola NAT in uscita.

   ```PowerShell
    $OutboundNAT = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRule
    $OutboundNAT.ResourceId = "onat1"
    
    $OutboundNAT.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRuleProperties
    $OutboundNAT.properties.frontendipconfigurations += $FrontEndIPConfig
    $OutboundNAT.properties.backendaddresspool = $BackEndAddressPool
    $OutboundNAT.properties.protocol = "ALL"

    $LoadBalancerProperties.OutboundNatRules += $OutboundNAT
   ```

3. Aggiungere l'oggetto del servizio di bilanciamento del carico nel controller di rete.

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

4. Seguire l'esempio seguente per aggiungere le interfacce di rete a cui si vuole fornire l'accesso a Internet.

## <a name="example-add-network-interfaces-to-the-back-end-pool"></a>Esempio: Aggiungere interfacce di rete al pool back-end
In questo esempio si aggiungono interfacce di rete al pool back-end.  È necessario ripetere questo passaggio per ogni interfaccia di rete in grado di elaborare le richieste effettuate all'indirizzo VIP. 

È anche possibile ripetere questo processo su una singola interfaccia di rete per aggiungerla a più oggetti del servizio di bilanciamento del carico. Ad esempio, se si dispone di un oggetto di bilanciamento del carico per un indirizzo VIP del server Web e di un oggetto del servizio di bilanciamento del carico separato per fornire NAT in uscita.
    
1. Ottenere l'oggetto del servizio di bilanciamento del carico contenente il pool back-end per aggiungere un'interfaccia di rete.

   ```PowerShell
   $lbresourceid = "LB2"
   $lb = get-networkcontrollerloadbalancer -connectionuri $uri -resourceID $LBResourceId -PassInnerException
   ```

2. Ottenere l'interfaccia di rete e aggiungere il pool backendaddress alla matrice loadbalancerbackendaddresspools.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -PassInnerException
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   ```  

3. Inserire l'interfaccia di rete per applicare la modifica. 

   ```PowerShell
   new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force -PassInnerException
   ``` 


## <a name="example-use-the-software-load-balancer-for-forwarding-traffic"></a>Esempio: Usare il software Load Balancer per l'invio del traffico
Se è necessario eseguire il mapping di un indirizzo IP virtuale a una singola interfaccia di rete in una rete virtuale senza definire singole porte, è possibile creare una regola di invio L3.  Questa regola trasmette tutto il traffico da e verso la macchina virtuale tramite l'indirizzo VIP assegnato contenuto in un oggetto PublicIPAddress.

Se è stato definito il VIP e DIP come stessa subnet, equivale a eseguire l'invio L3 senza NAT.

>[!NOTE]
>Questo processo non richiede la creazione di un oggetto del servizio di bilanciamento del carico.  L'assegnazione di PublicIPAddress all'interfaccia di rete è un numero sufficiente di informazioni per consentire al software Load Balancer di eseguire la propria configurazione.


1. Creare un oggetto IP pubblico che contenga l'indirizzo VIP.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.ipaddress = "10.127.134.7"
   $publicIPProperties.PublicIPAllocationMethod = "static"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Assegnare PublicIPAddress a un'interfaccia di rete.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```

## <a name="example-use-the-software-load-balancer-for-forwarding-traffic-with-a-dynamically-allocated-vip"></a>Esempio: Usare il Load Balancer software per l'invio del traffico con un indirizzo VIP allocato in modo dinamico
In questo esempio viene ripetuta la stessa azione dell'esempio precedente, ma viene automaticamente allocato l'indirizzo VIP dal pool di indirizzi VIP disponibile nel servizio di bilanciamento del carico invece di specificare un indirizzo IP specifico. 

1. Creare un oggetto IP pubblico che contenga l'indirizzo VIP.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.PublicIPAllocationMethod = "dynamic"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Eseguire una query sulla risorsa PublicIPAddress per determinare l'indirizzo IP assegnato.

   ```PowerShell
    (Get-NetworkControllerPublicIpAddress -ConnectionUri $uri -ResourceId "MyPIP").properties
   ```

   La proprietà IpAddress contiene l'indirizzo assegnato.  L'output sarà simile al seguente:
   ```
    Counters                 : {}
    ConfigurationState       :
    IpAddress                : 10.127.134.2
    PublicIPAddressVersion   : IPv4
    PublicIPAllocationMethod : Dynamic
    IdleTimeoutInMinutes     : 4
    DnsSettings              :
    ProvisioningState        : Succeeded
    IpConfiguration          :
    PreviousIpConfiguration  :
   ```
 
3. Assegnare PublicIPAddress a un'interfaccia di rete.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```
   ## <a name="example-remove-a-publicip-address-that-is-being-used-for-forwarding-traffic-and-return-it-to-the-vip-pool"></a>Esempio: Rimuovere un indirizzo IP pubblico usato per l'invio del traffico e restituirlo al pool VIP
   Questo esempio Mostra come rimuovere la risorsa PublicIPAddress creata dagli esempi precedenti.  Una volta rimosso il PublicIPAddress, il riferimento al PublicIPAddress verrà rimosso automaticamente dall'interfaccia di rete, il traffico verrà interrotto e l'indirizzo IP verrà restituito al pool di indirizzi VIP pubblici per il riutilizzo.  

4. Rimuovere IP pubblico

   ```PowerShell
   Remove-NetworkControllerPublicIPAddress -ConnectionURI $uri -ResourceId "MyPIP"
   ```

---


 