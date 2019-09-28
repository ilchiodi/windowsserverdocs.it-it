---
title: Usare le appliance di rete virtuali in una rete virtuale
description: In questo argomento si apprenderà come distribuire appliance virtuali di rete nelle reti virtuali tenant. È possibile aggiungere appliance virtuali di rete alle reti che eseguono funzioni di routing e di mirroring delle porte definite dall'utente.
manager: dougkim
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.prod: windows-server
ms.technology: networking-sdn
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 158183bab74e6e45c36c579f3259fc2095a939b5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406047"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Usare le appliance di rete virtuali in una rete virtuale

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento si apprenderà come distribuire appliance virtuali di rete nelle reti virtuali tenant. È possibile aggiungere appliance virtuali di rete alle reti che eseguono funzioni di routing e di mirroring delle porte definite dall'utente.

## <a name="types-of-network-virtual-appliances"></a>Tipi di appliance virtuali di rete

È possibile usare uno dei due tipi di appliance virtuali:

1. **Routing definito dall'utente** : sostituisce i router distribuiti nella rete virtuale con le funzionalità di routing dell'appliance virtuale.  Con il routing definito dall'utente, l'appliance virtuale viene usata come router tra le subnet virtuali nella rete virtuale.

2. **Mirroring delle porte** : tutto il traffico di rete in ingresso o in uscita dalla porta monitorata viene duplicato e inviato a un'appliance virtuale per l'analisi. 


## <a name="deploying-a-network-virtual-appliance"></a>Distribuzione di un'appliance virtuale di rete

Per distribuire un'appliance virtuale di rete, è necessario creare prima di tutto una macchina virtuale che contenga l'appliance, quindi connettere la VM alle subnet della rete virtuale appropriata. Per altre informazioni, vedere [creare una VM tenant e connettersi a una rete virtuale tenant o a una VLAN](Create-a-Tenant-VM.md).

Alcune Appliance richiedono più schede di rete virtuali. In genere, una scheda di rete dedicata alla gestione degli appliance mentre gli adapter aggiuntivi elaborano il traffico.  Se il dispositivo richiede più schede di rete, è necessario creare ogni interfaccia di rete nel controller di rete. È inoltre necessario assegnare un ID di interfaccia in ogni host per ognuno degli adapter aggiuntivi che si trovano in subnet virtuali diverse.

Dopo aver distribuito l'appliance virtuale di rete, è possibile usare l'appliance per il routing definito, il mirroring del porting o entrambi. 


## <a name="example-user-defined-routing"></a>Esempio: Routing definito dall'utente

Per la maggior parte degli ambienti sono necessarie solo le route di sistema già definite dal router distribuito della rete virtuale. Potrebbe tuttavia essere necessario creare una tabella di routing e aggiungere una o più route in casi specifici, ad esempio:

- Forzare il tunneling a Internet tramite la rete locale.
- Uso di appliance virtuali nell'ambiente.

Per questi scenari, è necessario creare una tabella di routing e aggiungere route definite dall'utente alla tabella. È possibile disporre di più tabelle di routing ed è possibile associare la stessa tabella di routing a una o più subnet. È possibile associare ogni subnet solo a una singola tabella di routing. Tutte le macchine virtuali in una subnet utilizzano la tabella di routing associata alla subnet.

Le subnet si basano sulle route di sistema fino a quando una tabella di routing non viene associata alla subnet. Una volta esistente un'associazione, il routing viene eseguito in base alla corrispondenza di prefisso più lunga (LPM) tra le route definite dall'utente e le route di sistema. Se è presente più di una route con la stessa corrispondenza LPM, la route definita dall'utente viene selezionata prima della route di sistema.
 
**Procedura**

1. Creare le proprietà della tabella di route, che contengono tutte le route definite dall'utente.<p>Le route di sistema sono comunque valide in base alle regole definite in precedenza.

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. Aggiungere una route alle proprietà della tabella di routing.<p>Tutte le route destinate alla subnet 12.0.0.0/8 vengono indirizzate all'appliance virtuale in 192.168.1.10. Il dispositivo deve disporre di una scheda di rete virtuale collegata alla rete virtuale con l'indirizzo IP assegnato a un'interfaccia di rete.

   ```PowerShell
    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route
   ```
   >[!TIP]
   >Se si desidera aggiungere altre route, ripetere questo passaggio per ogni route che si desidera definire.

3. Aggiungere la tabella di routing al controller di rete.

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. Applicare la tabella di routing alla subnet virtuale.<p>Quando si applica la tabella di route alla subnet virtuale, la prima subnet virtuale nella rete Tenant1_Vnet1 usa la tabella di route. È possibile assegnare la tabella di route a tutte le subnet nella rete virtuale desiderata.

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

Non appena si applica la tabella di routing alla rete virtuale, il traffico viene inoltrato all'appliance virtuale. È necessario configurare la tabella di routing nell'appliance virtuale per l'inoltro del traffico, in modo appropriato per l'ambiente in uso.

## <a name="example-port-mirroring"></a>Esempio: Mirroring delle porte

In questo esempio viene configurato il traffico per MyVM_Ethernet1 per eseguire il mirroring di Appliance_Ethernet1.  Si presuppone che siano state distribuite due macchine virtuali, una come appliance e l'altra come macchina virtuale da monitorare con il mirroring. 

Il dispositivo deve disporre di una seconda interfaccia di rete per la gestione. Dopo aver abilitato il mirroring come destinazione in Appliciance_Ethernet1, non riceve più il traffico destinato all'interfaccia IP configurata.


**Procedura**

1. Ottenere la rete virtuale in cui si trovano le macchine virtuali.

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. Ottenere le interfacce di rete del controller di rete per l'origine e la destinazione del mirroring.

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. Creare un oggetto serviceinsertionproperties per contenere le regole di mirroring delle porte e l'elemento che rappresenta l'interfaccia di destinazione.

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. Creare un oggetto serviceinsertionrules per contenere le regole per le quali è necessario trovare una corrispondenza affinché il traffico venga inviato all'appliance.<p>Le regole definite di seguito corrispondono a tutto il traffico, sia in ingresso che in uscita, che rappresenta un mirror tradizionale.  È possibile modificare queste regole se si è interessati al mirroring di una porta specifica o di un'origine/destinazione specifica.

   ```PowerShell
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
   ```

5. Creare un oggetto serviceinsertionelements per contenere l'interfaccia di rete del dispositivo con mirroring.

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. Aggiungere l'oggetto inserimento del servizio nel controller di rete.<p>Quando si esegue questo comando, tutto il traffico verso l'interfaccia di rete dell'appliance specificato nel passaggio precedente si arresta.

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. Aggiornare l'interfaccia di rete dell'origine per il mirroring.

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

Dopo aver completato questi passaggi, l'interfaccia Appliance_Ethernet1 riflette il traffico dall'interfaccia MyVM_Ethernet1.
 
---