---
title: Usare le appliance di rete virtuali in una rete virtuale
description: In questo argomento descrive come distribuire Appliance virtuali di rete nelle reti virtuali tenant. È possibile aggiungere Appliance virtuali di rete per reti a cui eseguono il routing definito dall'utente e funzioni di mirroring delle porte.
manager: dougkim
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
ms.date: 08/30/2018
ms.openlocfilehash: e715a782651a5b9867f3b45251fd6ea6e4a9e4f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847372"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Usare le appliance di rete virtuali in una rete virtuale

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento descrive come distribuire Appliance virtuali di rete nelle reti virtuali tenant. È possibile aggiungere Appliance virtuali di rete per reti a cui eseguono il routing definito dall'utente e funzioni di mirroring delle porte.

## <a name="types-of-network-virtual-appliances"></a>Tipi di Appliance virtuali di rete

È possibile usare uno dei due tipi di Appliance virtuali:

1. **Routing definito dall'utente** -sostituisce router distribuito nella rete virtuale con le funzionalità di routing dell'appliance virtuale.  Con il routing definito dall'utente, l'appliance virtuale viene usata come un router tra le subnet virtuali nella rete virtuale.

2. **Il mirroring delle porte** : tutto sta immettendo il traffico di rete o lasciare la porta monitorata viene duplicato e inviato a un'appliance virtuale per l'analisi. 


## <a name="deploying-a-network-virtual-appliance"></a>Distribuzione di un'appliance virtuale di rete

Per distribuire un'appliance virtuale di rete, è necessario innanzitutto creare una macchina virtuale che contiene l'appliance e quindi connettere la macchina virtuale per la subnet della rete virtuale appropriato. Per altre informazioni, vedere [creare una macchina virtuale Tenant e connessione a una rete virtuale del Tenant o VLAN](Create-a-Tenant-VM.md).

Alcuni dispositivi richiedono più schede di rete virtuale. In genere, una scheda di rete dedicata per la gestione di appliance mentre altri adapter elaborano il traffico.  Se il dispositivo richiede più schede di rete, è necessario creare ogni interfaccia di rete nel Controller di rete. È anche necessario assegnare un ID di interfaccia in ogni host per ognuna delle schede aggiuntive che si trovano in subnet distinte.

Dopo aver distribuito l'appliance virtuale di rete, è possibile usare l'appliance per il routing definito, il mirroring del porting o entrambi. 


## <a name="example-user-defined-routing"></a>Esempio: Routing definito dall'utente

Per la maggior parte degli ambienti, è necessario solo le route di sistema già definite da router distribuito della rete virtuale. Tuttavia, potrebbe essere necessario creare una tabella di routing e aggiungere una o più route in casi specifici, ad esempio:

- Il tunneling forzato a Internet tramite la rete locale.
- Uso di Appliance virtuali nell'ambiente in uso.

Per questi scenari, è necessario creare una tabella di routing e aggiungere le route definite dall'utente alla tabella. È possibile avere più tabelle di routing ed è possibile associare la stessa tabella di routing a una o più subnet. È possibile associare solo ogni subnet per una singola tabella di routing. Tutte le macchine virtuali in uso una subnet la tabella di routing associato alla subnet.

Le subnet si basano su route predefinite fino a quando non viene associata alla subnet di una tabella di routing. Dopo aver creato un'associazione, il routing viene effettuato in base su più lunga prefisso (LPM) tra le route definite dall'utente e le route di sistema. Se è presente più di una route con la stessa corrispondenza LPM, quindi la route definita dall'utente è selezionata per primo - prima la route di sistema.
 
**Procedura:**

1. Creare la route le proprietà di tabella che contiene tutte le route definite dall'utente.<p>Route di sistema vengono mantenuti in base alle regole definite sopra.

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. Aggiungere una route per le proprietà delle tabelle di routing.<p>Tutte le route destinate 12.0.0.0/8 subnet viene indirizzata all'Appliance virtuale in 192.168.1.10. L'appliance deve avere una scheda di rete virtuali collegata alla rete virtuale con tale indirizzo IP assegnato a un'interfaccia di rete.

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

3. Aggiungere la tabella di routing al Controller di rete.

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. Applicare la tabella di routing alla subnet virtuali.<p>Quando si applica la tabella di route alla subnet virtuali, la prima subnet virtuale nella rete Tenant1_Vnet1 Usa la tabella di route. Come si desidera, è possibile assegnare la tabella di route per il maggior numero di subnet nella rete virtuale.

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

Non appena si applica la tabella di routing nella rete virtuale, il traffico viene inoltrato all'Appliance virtuale. È necessario configurare la tabella di routing nell'appliance virtuale di inoltrare il traffico, in modo appropriato per l'ambiente.

## <a name="example-port-mirroring"></a>Esempio: Mirroring delle porte

In questo esempio, configurare il traffico per MyVM_Ethernet1 mirror Appliance_Ethernet1.  Si presuppone di aver distribuito due macchine virtuali, uno come l'appliance e l'altro come macchina virtuale per il monitoraggio con mirroring. 

L'appliance deve avere una seconda interfaccia di rete per la gestione. Dopo aver abilitato il mirroring come una destinazione Appliciance_Ethernet1, non riceva più il traffico destinato per l'interfaccia IP configurato non esiste.


**Procedura:**

1. Ottiene la rete virtuale in cui si trovano le macchine virtuali.

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. Ottenere le interfacce di rete del Controller di rete per l'origine e la destinazione del mirroring.

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. Creare un oggetto serviceinsertionproperties per contenere le regole e l'elemento che rappresenta l'interfaccia di destinazione di mirroring delle porte.

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. Creare un oggetto serviceinsertionrules per contenere le regole che devono essere corrisposte affinché il traffico da inviare al dispositivo.<p>Le regole definite sotto corrispondenza tutto il traffico, sia in ingresso e in uscita, che rappresenta un mirror tradizionale.  Se si è interessati a mirroring, una porta specifica o origini/destinazioni specifiche, è possibile modificare queste regole.

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

5. Creare un oggetto serviceinsertionelements per contenere l'interfaccia di rete dell'appliance con mirroring.

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. Aggiungere l'oggetto di inserimento del servizio nel Controller di rete.<p>Quando si esegue questo comando, tutto il traffico all'appliance di rete interfaccia specificata in Arresta il passaggio precedente.

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. Aggiornare l'interfaccia di rete dell'origine da sottoporre a mirroring.

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

Dopo aver completato questi passaggi, l'interfaccia Appliance_Ethernet1 rispecchia il traffico dall'interfaccia MyVM_Ethernet1.
 
---