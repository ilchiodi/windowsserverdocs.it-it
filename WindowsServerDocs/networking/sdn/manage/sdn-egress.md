---
title: Misurazione in uscita nella rete virtuale
description: 'Un aspetto fondamentale di monetizzazione rete cloud è in uscita della larghezza di banda di rete. Ad esempio: dati in uscita trasferisce il modello di business In Microsoft Azure. Dati in uscita viene addebitati in base alla quantità totale di dati spostati esternamente Data Center di Azure tramite Internet in un determinato ciclo di fatturazione.'
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: 50aee16b0b5797f28ebcdf61494b09669699873f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446326"
---
# <a name="egress-metering-in-a-virtual-network"></a>Misurazione in uscita in una rete virtuale

>Si applica a: Windows Server 2019


Un aspetto fondamentale di monetizzazione rete cloud è in grado di fatturare dall'utilizzo della larghezza di banda di rete. Dati in uscita viene addebitati in base alla quantità totale di dati spostati esternamente il data center tramite Internet in un determinato ciclo di fatturazione.

Controllo del traffico in uscita per il traffico di rete SDN in Windows Server 2019 offre la possibilità di offrire i contatori di utilizzo per i trasferimenti di dati in uscita. Il traffico di rete che esce da ogni rete virtuale, ma restano all'interno dei data center possono da rilevati separatamente, in modo che è possibile escludere dai calcoli di fatturazione. Per gli indirizzi IP di destinazione che non sono inclusi in uno degli intervalli di indirizzi non fatturati i pacchetti vengono rilevati fatturati i trasferimenti di dati in uscita.

## <a name="virtual-network-unbilled-address-ranges-whitelist-of-ip-ranges"></a>Intervalli (elenco di intervalli di indirizzi IP consentiti) di indirizzi di rete virtuale non fatturati

È possibile trovare gli intervalli di indirizzi non fatturati con le **UnbilledAddressRanges** proprietà di una rete virtuale esistente. Per impostazione predefinita, non sono aggiunti gli intervalli di indirizzi.

   ```PowerShell
   import-module NetworkController
   $uri = "https://sdn.contoso.com"

   (Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties
   ```

L'output sarà simile al seguente:
   ```
    AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
    DhcpOptions            :
    UnbilledAddressRanges  :
    ConfigurationState     :
    ProvisioningState      : Succeeded
    Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                         29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
    VirtualNetworkPeerings :
    EncryptionCredential   :
    LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>Esempio: Gestire gli intervalli di indirizzi resoconti di una rete virtuale

È possibile gestire il set di prefissi di subnet IP da escludere dall'uscita fatturato misurazione impostando il **UnbilledAddressRange** proprietà di una rete virtuale.  Tutto il traffico inviato da interfacce di rete nella rete virtuale con un indirizzo IP di destinazione che corrisponde a uno dei prefissi di non essere incluso nella proprietà BilledEgressBytes.

1.  Aggiorna il **UnbilledAddressRanges** proprietà contenga le subnet che non verranno fatturate per l'accesso.

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >Se si aggiungono più subnet IP, usare una virgola tra ogni subnet IP.  Non includere spazi prima o dopo la virgola.

2.  Aggiornare la risorsa rete virtuale con modificato **UnbilledAddressRanges** proprietà.

    ```PowerShell
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "VNet1" -Properties $unbilled.Properties -PassInnerException
    ```

    L'output sarà simile al seguente:
    ```
    Confirm
    Performing the operation 'New-NetworkControllerVirtualNetwork' on entities of type
    'Microsoft.Windows.NetworkController.VirtualNetwork' via
    'https://sdn.contoso.com/networking/v3/virtualNetworks/VNet1'. Are you sure you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y


~~~
Tags             :
ResourceRef      : /virtualNetworks/VNet1
InstanceId       : 29654b0b-9091-4bed-ab01-e172225dc02d
Etag             : W/"6970d0a3-3444-41d7-bbe4-36327968d853"
ResourceMetadata :
ResourceId       : VNet1
Properties       : Microsoft.Windows.NetworkController.VirtualNetworkProperties
```
~~~


3. Check the Virtual Network to see the configured **UnbilledAddressRanges**.

   ```PowerShell
   (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
   ```

   Your output will now look similar to this:
   ```
   AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
   DhcpOptions            :
   UnbilledAddressRanges  : 10.10.2.0/24,192.168.2.0/24
   ConfigurationState     :
   ProvisioningState      : Succeeded
   Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                        29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
   VirtualNetworkPeerings :
   EncryptionCredential   :
   LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```

## Check the billed the unbilled egress usage of a virtual network

After you configure the **UnbilledAddressRanges** property, you can check the billed and unbilled egress usage of each subnet within a virtual network. Egress traffic updates every four minutes with the total bytes of the billed and unbilled ranges.

The following properties are available for each virtual subnet:

-   **UnbilledEgressBytes** shows the number of unbilled bytes sent by network interfaces connected to this virtual subnet. Unbilled bytes are bytes sent to address ranges that are part of the **UnbilledAddressRanges** property of the parent virtual network.

-   **BilledEgressBytes** shows Number of billed bytes sent by network interfaces connected to this virtual subnet. Billed bytes are bytes sent to address ranges that are not part of the **UnbilledAddressRanges** property of the parent virtual network.

Use the following example to query egress usage:

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

Your output will look similar to this:
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```


---