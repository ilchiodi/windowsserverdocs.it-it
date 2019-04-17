---
title: Configurare il bilanciamento del carico Software per il bilanciamento del carico e Network Address Translation (NAT)
description: Questo argomento fa parte della Guida alla rete definita dal Software su come gestire carichi di lavoro Tenant e reti virtuali in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f0393db564061caa0bc8f18b1d623f24749b46c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>Configurare il bilanciamento del carico Software per il bilanciamento del carico e Network Address Translation (NAT)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come usare il bilanciamento del carico di rete definita dal Software \(SDN\) software \(SLB\) per fornire conversione indirizzi di rete in uscita NAT, NAT in entrata, o bilanciamento del carico tra più istanze di un'applicazione.

In questo argomento contiene le sezioni seguenti.

- [Panoramica del servizio di bilanciamento del carico software](#bkmk_slbover)
- [Esempio: Creare un VIP pubblico per il bilanciamento del carico a un pool di due macchine virtuali in una rete virtuale](#bkmk_publicvip)
- [Esempio: Utilizzo SLB per NAT in uscita](#bkmk_obnat)
- [Esempio: Aggiungere le interfacce di rete al pool di back-end](#bkmk_backend)
- [Esempio Utilizzare ad il bilanciamento del carico Software per l'inoltro di traffico](#bkmk_forward)

## <a name="bkmk_slbover"></a>Panoramica del servizio di bilanciamento del carico software

\(SLB\) il bilanciamento del carico Software di SDN offre prestazioni di rete e la disponibilità elevata per le applicazioni. Si tratta di un livello di 4 \ (TCP, UDP\) bilanciamento del carico che distribuisce il traffico in ingresso tra le istanze del servizio integrità in servizi cloud o macchine virtuali definite in un set di bilanciamento del carico.

È possibile configurare SLB per eseguire le operazioni seguenti.

* Carico bilanciare il traffico in ingresso a una rete virtuale per le macchine virtuali \(VMs\) esterno. Si tratta del bilanciamento del carico di indirizzi VIP pubblica.
* Carico bilanciare il traffico in ingresso tra le macchine virtuali in una rete virtuale, tra le macchine virtuali nei servizi cloud o tra computer locali e le macchine virtuali in una rete virtuale cross-premise. 
* Inoltrare il traffico di rete di macchina virtuale dalla rete virtuale esterna a destinazioni con NAT (NAT).  Questa operazione è detta NAT in uscita.
* Inoltrare il traffico esterno a una macchina virtuale specifica.  Questa operazione è detta NAT in entrata.

>[!IMPORTANT]
>Un problema noto impedisce che gli oggetti di bilanciamento del carico nel modulo di Windows PowerShell NetworkController funziona correttamente in Windows Server 2016 5. La soluzione consiste nell'usare tabelle hash dinamico e Invoke-WebRequest invece. Questo metodo è illustrato negli esempi seguenti.


## <a name="bkmk_publicvip"></a>Esempio: Creare un VIP pubblico per il bilanciamento del carico a un pool di due macchine virtuali in una rete virtuale

È possibile utilizzare questo esempio mostra come creare un oggetto di bilanciamento del carico con un VIP pubblico e due macchine virtuali come membri pool per soddisfare le richieste per l'indirizzo VIP.  Questo codice di esempio aggiunge anche un probe di stato HTTP per rilevare se uno dei membri pool diventa inattiva.

###<a name="step-1-prepare-the-load-balancer-object"></a>Passaggio 1: Preparare l'oggetto di bilanciamento del carico
È possibile utilizzare l'esempio seguente per preparare l'oggetto di bilanciamento del carico.

    $lbresourceId = "LB2"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

###<a name="step-2-assign-a-front-end-ip"></a>Passaggio 2: Assegnare un indirizzo IP front-end
IP front-end è comunemente noti come un IP virtuale (VIP).  L'indirizzo VIP deve essere prese da un indirizzo IP non utilizzato in uno dei Pool IP che sono state assegnate in precedenza per Gestione bilanciamento carico di rete logica.

Per assegnare un indirizzo IP front-end, è possibile utilizzare l'esempio seguente.

    $vipip = "10.127.132.5"
    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

###<a name="step-3-allocate-a-backend-address-pool"></a>Passaggio 3: Allocare un pool di indirizzi back-end
Il pool di indirizzi back-end contiene gli indirizzi IP dinamici (DIP) che costituiscono i membri del set di macchine virtuali con carico bilanciato. In questo passaggio è solo allocare il pool. le configurazioni IP vengono aggiunti in un passaggio successivo.

È possibile utilizzare l'esempio seguente per allocare un pool di indirizzi di back-end.
 
    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-4-define-a-health-probe"></a>Passaggio 4: Definire un probe dello stato
Probe dello stato vengono utilizzati dal servizio di bilanciamento del carico per determinare lo stato dei membri pool back-end. Con questo esempio, si definisce un probe HTTP che esegue una query per il RequestPath di "/ health.htm".  La query viene eseguita ogni 5 secondi, come specificato dalla proprietà IntervalInSeconds.

Probe di stato deve ricevere un codice di risposta HTTP 200 per query consecutive 11 del probe da prendere in considerazione l'IP back-end sia integro. Se l'IP back-end non è integro, il bilanciamento del carico non invia il traffico IP.

>[!Note]
>È importante che qualsiasi elenchi di controllo che si applicano alle IP back-end non blocchino il traffico verso o dal primo indirizzo IP della subnet, perché è il punto di origine per i probe.

È possibile utilizzare l'esempio seguente per definire un probe dello stato.
 
    $lbprobe = @{}
    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$lbresourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties = @{}
    $lbprobe.properties.protocol = "HTTP"
    $lbprobe.properties.port = "80"
    $lbprobe.properties.RequestPath = "/health.htm"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11
    $lbproperties.probes += $lbprobe

###<a name="step-5-define-a-load-balancing-rule"></a>Passaggio 5: Definire un regola di bilanciamento del carico
Questa regola di bilanciamento del carico definisce come il traffico che arriva a IP front-end di essere inviati a IP back-end.  In questo esempio, il traffico TCP sulla porta 80 viene inviato al pool di back-end.

È possibile utilizzare l'esempio seguente per definire un regola di bilanciamento del carico.

    $lbrule = @{}
    $lbrule.ResourceId = "webserver1"
    $lbrule.properties = @{}
    $lbrule.properties.FrontEndIPConfigurations = @()
    $lbrule.properties.FrontEndIPConfigurations += $fe
    $lbrule.properties.backendaddresspool = $backend 
    $lbrule.properties.protocol = "TCP"
    $lbrule.properties.frontendPort = 80
    $lbrule.properties.Probe = $lbprobe
    $lbproperties.loadbalancingRules += $lbrule

###<a name="step-6-add-the-load-balancer-configuration-to-network-controller"></a>Passaggio 6: Aggiungere la configurazione di bilanciamento del carico al Controller di rete
Finora in questo esempio, tutti gli oggetti creati sono nella memoria della sessione di Windows PowerShell. Questo passaggio consente di aggiungere gli oggetti al Controller di rete.

È possibile utilizzare l'esempio seguente per aggiungere la configurazione di bilanciamento del carico al Controller di rete.

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

Dopo questo passaggio è necessario seguire l'esempio seguente per aggiungere le interfacce di rete per questo pool back-end.

## <a name="bkmk_obnat"></a>Esempio: Utilizzo SLB per NAT in uscita

È possibile utilizzare questo esempio mostra come configurare SLB con un pool di back-end per fornire funzionalità NAT in uscita per una macchina virtuale in spazio di indirizzi privato della rete virtuale raggiungere in uscita a internet.

###<a name="step-1-create-the-loadbalancer-properties-front-end-ip-and-backend-pool"></a>Passaggio 1: Creare le proprietà bilanciatore del carico, per IP front-end e back-end Pool.
È possibile utilizzare l'esempio seguente per creare le proprietà bilanciatore del carico, per IP front-end e back-end Pool.

    $lbresourceId = "OutboundNATMembers"
    $vipip = "10.127.132.7"

    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-2-define-the-outbound-nat-rule"></a>Passaggio 2: Definire la regola NAT in uscita
È possibile utilizzare l'esempio seguente per definire la regola NAT in uscita. 

    $onat = @{}
    $onat.ResourceId = "onat1"
    $onat.properties = @{}
    $onat.properties.frontendipconfigurations = @()
    $onat.properties.frontendipconfigurations += $fe
    $onat.properties.backendaddresspool = $backend
    $onat.properties.protocol = "ALL"
    $lbproperties.OutboundNatRules += $onat

###<a name="step-3-add-the-load-balancer-object-in-network-controller"></a>Passaggio 3: Aggiungere l'oggetto del servizio di bilanciamento di carico in Controller di rete
È possibile utilizzare l'esempio seguente per aggiungere l'oggetto del servizio di bilanciamento di carico in Controller di rete.

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

Nel passaggio successivo, è possibile aggiungere le interfacce di rete a cui si desidera fornire l'accesso a internet.

## <a name="bkmk_backend"></a>Esempio: Aggiungere le interfacce di rete al pool di back-end
È possibile utilizzare questo esempio mostra come aggiungere le interfacce di rete al pool di back-end.

È necessario ripetere questo passaggio per ogni interfaccia di rete in grado di elaborare le richieste che vengono apportate all'indirizzo VIP. È anche possibile ripetere questo processo in una singola interfaccia di rete per aggiungerlo a più oggetti del servizio di bilanciamento di carico. Ad esempio, se si dispone di un oggetto di bilanciamento del carico per un Server Web VIP e un oggetto del servizio di bilanciamento carico separato per fornire NAT in uscita.
    
### <a name="step-1-get-the-load-balancer-object-containing-the-back-end-pool-to-which-you-will-add-a-network-interface"></a>Passaggio 1: Ottenere l'oggetto del servizio di bilanciamento carico che contiene il pool di back-end a cui si desidera aggiungere un'interfaccia di rete
È possibile utilizzare l'esempio seguente per recuperare l'oggetto di bilanciamento del carico.

    $lbresourceid = "LB2"
    $lb = (Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Get" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -DisableKeepAlive -UseBasicParsing).content | convertfrom-json 

### <a name="step-2-get-the-network-interface-and-add-the-backendaddress-pool-to-the-loadbalancerbackendaddresspools-array"></a>Passaggio 2: Ottenere l'interfaccia di rete e aggiungere il pool backendaddress all'array di loadbalancerbackendaddresspools.
È possibile utilizzare l'esempio seguente per ottenere l'interfaccia di rete e aggiungere il pool backendaddress all'array di loadbalancerbackendaddresspools.

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    
### <a name="step-3-put-the-network-interface-to-apply-the-change"></a>Passaggio 3: Inserire l'interfaccia di rete per applicare la modifica
È possibile utilizzare l'esempio seguente per inserire l'interfaccia di rete per applicare la modifica.

    new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force
 
## <a name="bkmk_forward"></a>Esempio Utilizzare ad il bilanciamento del carico Software per l'inoltro di traffico
Se è necessario eseguire il mapping di un indirizzo IP virtuale a una singola interfaccia di rete in una rete virtuale senza definire singole porte, è possibile creare una regola di inoltro L3.  Questa regola inoltra tutto il traffico da e verso la macchina virtuale tramite l'indirizzo VIP assegnato, che deve essere incluso in un oggetto PublicIPAddress.

Se l'indirizzo VIP e DIP sono definite come della stessa subnet, quindi questa operazione equivale a eseguire l'inoltro L3 senza NAT.

>[!NOTE]
>Questo processo non richiede la creazione di un oggetto di bilanciamento del carico.  L'assegnazione di PublicIPAddress all'interfaccia di rete è informazioni sufficienti per il bilanciamento del carico Software eseguire la configurazione.

###<a name="step-1-create-a-public-ip-object-to-contain-the-vip"></a>Passaggio 1: Creare un oggetto IP pubblico per contenere l'indirizzo VIP
È possibile utilizzare l'esempio seguente per creare un oggetto IP pubblico.

    $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
    $publicIPProperties.ipaddress = "10.127.132.6"
    $publicIPProperties.PublicIPAllocationMethod = "static"
    $publicIPProperties.IdleTimeoutInMinutes = 4
    $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri

####<a name="step-2-assign-the-publicipaddress-to-a-network-interface"></a>Passaggio 2: Assegnare la PublicIPAddress a un'interfaccia di rete
Per assegnare il PublicIPAddress a un'interfaccia di rete, è possibile utilizzare l'esempio seguente.

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
    New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties



 