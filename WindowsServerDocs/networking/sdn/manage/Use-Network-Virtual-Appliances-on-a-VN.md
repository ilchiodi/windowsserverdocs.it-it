---
title: Utilizzare dispositivi di rete virtuali in una rete virtuale
description: Questo argomento fa parte della Guida alla rete definita dal Software su come gestire carichi di lavoro Tenant e reti virtuali in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: db46189931263d230f013431f319eb2497589dee
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Utilizzare dispositivi di rete virtuali in una rete virtuale

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come distribuire Appliance di rete virtuali nel tenant di reti virtuali.

È possibile aggiungere dispositivi di rete virtuali alle reti che eseguono il routing definita dall'utente e le funzioni di mirroring delle porte.

In questo argomento contiene le sezioni seguenti.

- [Tipi di accessori di rete virtuale](#bkmk_types)
- [Distribuzione di un dispositivo di rete virtuale](#bkmk_deploy)
- [Esempio: Definiti dall'utente Routing](#bkmk_routing)
- [Esempio: Il mirroring delle porte](#bkmk_port)

## <a name="bkmk_types"></a>Tipi di accessori di rete virtuale

Esistono due tipi di Appliance virtuali che è possibile utilizzare in reti virtuali:

1. **Definita dall'utente routing**. Routing definita dall'utente sostituisce router distribuito nella rete virtuale con le funzionalità di dispositivo virtuale routing.  Il dispositivo virtuale definito dall'utente routing, viene usato come router tra le subnet virtuali nella rete virtuale.
2. **Il mirroring delle porte**. Con il mirroring delle porte, tutto il traffico di rete che entra o lasciare la porta monitorata viene duplicato e inviato a un accessorio virtuale per l'analisi. 
## <a name="bkmk_deploy"></a>Distribuzione di un dispositivo di rete virtuale

Per distribuire un accessorio virtuale, è necessario creare prima di tutto una macchina virtuale (VM) che contiene il dispositivo e quindi connettere la macchina virtuale per le subnet di rete virtuale appropriato.

>[!NOTE]
>Per ulteriori informazioni, vedere [creare una macchina virtuale Tenant e connettersi a una rete virtuale del Tenant o VLAN](Create-a-Tenant-VM.md)

Alcuni accessori richiedono più schede di rete virtuale. In genere una scheda di rete dedicata per la gestione del dispositivo mentre schede aggiuntive vengono utilizzate per l'elaborazione del traffico. 

Se il dispositivo richiede più schede di rete, è necessario creare ogni interfaccia di rete nel Controller di rete. 

È inoltre necessario assegnare un ID di interfaccia in ogni host per ciascuna delle schede aggiuntive che si trovano in diverse subnet virtuali.

Dopo aver completato la distribuzione accessorio virtuale di rete, è possibile utilizzare il dispositivo per il routing definita dall'utente, il mirroring delle porte o entrambi.

##<a name="bkmk_routing"></a>Esempio: Definiti dall'utente Routing

Per la maggior parte degli ambienti è necessario solo le route di sistema già definite dal router distribuito della rete virtuale. Tuttavia, potrebbe essere necessario creare una tabella di route e aggiungere uno o più route in casi specifici, ad esempio:

* Il tunneling forzato a Internet tramite la rete locale.
* Uso dei dispositivi virtuali nell'ambiente in uso.

Per questi scenari, è necessario creare una tabella di route e aggiungere route definita dall'utente alla tabella. È possibile avere più tabelle di route e la stessa tabella di route può essere associata a una o più subnet. 

Ogni subnet solo può essere associata a una tabella di route singolo. Tutte le macchine virtuali in una subnet usano la tabella di route associato alla subnet in questione.

Subnet si basano sulle route di sistema fino a quando non è associata alla subnet di una tabella di route. Dopo che è presente un'associazione, il routing viene eseguito in base su più lunga del prefisso corrispondenza (LPM) tra sia route definita dall'utente e le route di sistema. 

Se è presente più di una route con la stessa corrispondenza LPM, quindi la route predefinita utente è selezionata per primo - prima la route di sistema. 

###<a name="step-1-create-the-route-table-properties"></a>Passaggio 1: Creare la route proprietà tabella

Questa tabella di routing contiene tutte le route definita dall'utente.  Route di sistema saranno ancora validi sulla base delle regole definite in precedenza.

È possibile utilizzare i seguenti comandi di esempio per creare le proprietà di tabella di route.

    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties

###<a name="step-2-add-a-route-to-the-route-table-properties"></a>Passaggio 2: Aggiungere una route per le proprietà della tabella di route

Questa route indica che deve ottenere inviato tutto il traffico destinato alla subnet 12.0.0.0/8 l'accessorio virtuale 192.168.1.10 routing.  È importante che il dispositivo dispone di una scheda di rete virtuali collegata alla rete virtuale con tale indirizzo IP assegnato a un'interfaccia di rete.

È possibile utilizzare i seguenti comandi di esempio per aggiungere una route per le proprietà della tabella di route.

    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route

È possibile aggiungere route aggiuntive, ripetere questo passaggio per ogni route che si desidera definire.
s
###<a name="step-3-add-the-route-table-to-network-controller"></a>Passaggio 3: Aggiungere la tabella di route al Controller di rete
È possibile utilizzare i seguenti comandi di esempio per aggiungere la tabella di route al Controller di rete.

    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties

###<a name="step-4-apply-the-route-table-to-the-virtual-subnet"></a>Passaggio 4: Applicare la tabella di route per la subnet virtuale
 
Quando si applica la tabella di route per la subnet virtuale, la subnet virtuale prima della rete Tenant1_Vnet1 Usa la tabella di route. È possibile assegnare la tabella di route per il numero delle subnet della rete virtuale che si desidera.

È possibile utilizzare i seguenti comandi di esempio per applicare la tabella di route per la subnet virtuale.

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid

Non appena si applica la tabella di route alla rete virtuale, viene inoltrato il traffico all'accessorio virtuale. È necessario configurare la tabella di routing nell'accessorio per inoltrare il traffico, in modo appropriato per l'ambiente virtuale.

##<a name="bkmk_port"></a>Esempio: Il mirroring delle porte

Questo esempio consente di configurare il traffico del MyVM_Ethernet1 in modo che il traffico viene eseguito il mirroring Appliance_Ethernet1.

Questo esempio si presuppone che due macchine virtuali, uno come il dispositivo e l'altro della macchina virtuale da monitorare con mirroring è già stato distribuito.

È importante che il dispositivo ha un'interfaccia di rete seconda per la gestione perché dopo il mirroring è abilitato come una destinazione nel Appliance_Ethernet1, non riceverà il traffico destinato per l'interfaccia IP è configurato.

###<a name="step-1-get-the-virtual-network-on-which-your-vms-are-located"></a>Passaggio 1: Ottenere la rete virtuale in cui si trovano nelle macchine virtuali
È possibile utilizzare il comando seguente per ottenere la rete virtuale.

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"

###<a name="step-2-get-the-network-controller-network-interfaces-for-the-mirroring-source-and-destination"></a>Passaggio 2: Ottenere le interfacce di rete Controller di rete per l'origine e destinazione mirroring
È possibile utilizzare i seguenti comandi di esempio per ottenere le interfacce di rete Controller di rete per l'origine e destinazione mirroring.

    $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
    $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-3-create-a-serviceinsertionproperties-object-to-contain-the-port-mirroring-rules-and-the-element-which-represents-the-destination-interface"></a>Passaggio 3: Creare un oggetto serviceinsertionproperties per contenere la porta mirroring regole e l'elemento che rappresenta l'interfaccia di destinazione
È possibile utilizzare i seguenti comandi di esempio per creare un oggetto di destinazione serviceinsertionproperties.

    $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
    $portMirror.Priority = 1

###<a name="step-4-create-a-serviceinsertionrules-object-to-contain-the-rules-that-must-be-matched-in-order-for-the-traffic-to-be-sent-to-the-appliance"></a>Passaggio 4: Creare un oggetto serviceinsertionrules per contenere le regole che devono corrispondere in modo che il traffico da inviare all'accessorio

Le regole definite sotto corrispondenza tutto il traffico, sia in ingresso e in uscita, che rappresenta un mirror tradizionale.  Se sei interessato a mirroring una porta specifica o origine/a destinazioni specifiche, è possibile modificare queste regole.

È possibile utilizzare i seguenti comandi di esempio per creare un oggetto serviceinsertionproperties.

    $portmirror.ServiceInsertionRules = [Microsoft.Windows.NetworkController.ServiceInsertionRule[]]::new(1)

    $portmirror.ServiceInsertionRules[0] = [Microsoft.Windows.NetworkController.ServiceInsertionRule]::new()
    $portmirror.ServiceInsertionRules[0].ResourceId = "Rule1"
    $portmirror.ServiceInsertionRules[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionRuleProperties]::new()

    $portmirror.ServiceInsertionRules[0].Properties.Description = "Port Mirror Rule"
    $portmirror.ServiceInsertionRules[0].Properties.Protocol = "All"
    $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeStart = "0"
    $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeEnd = "65535"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeStart = "0"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeEnd = "65535"
    $portmirror.ServiceInsertionRules[0].Properties.SourceSubnets = "*"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationSubnets = "*"

###<a name="step-5-create-a-serviceinsertionelements-object-to-contain-the-network-interface-of-the-appliance-you-are-mirroring-to"></a>Passaggio 5: Creare un oggetto serviceinsertionelements per contenere l'interfaccia di rete del dispositivo a che sono il mirroring
È possibile utilizzare i seguenti comandi di esempio per creare un oggetto serviceinsertionelements interfaccia di rete.

    $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

    $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
    $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
    $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

    $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
    $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
    $portmirror.ServiceInsertionElements[0].Properties.Order = 1

###<a name="step-6-add-the-service-insertion-object-in-network-controller"></a>Passaggio 6: Aggiungere l'oggetto di inserimento del servizio nel Controller di rete
Quando si esegue questo comando, si interrompe tutto il traffico all'interfaccia di rete accessorio specificato nel passaggio precedente.

È possibile utilizzare i seguenti comandi di esempio per aggiungere l'oggetto di inserimento del servizio nel Controller di rete.

    $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"

###<a name="step-7-update-the-network-interface-of-the-source-to-be-mirrored"></a>Passaggio 7: Aggiornare l'interfaccia di rete di origine viene eseguito il mirroring
È possibile utilizzare i seguenti comandi di esempio per aggiornare l'interfaccia di rete.

    $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
    $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId

Dopo aver completato questi passaggi, il traffico dall'interfaccia MyVM_Ethernet1 viene eseguito il mirroring dall'interfaccia Appliance_Ethernet1.
 
