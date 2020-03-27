---
title: Misurazione in uscita nella rete virtuale
description: Un aspetto fondamentale della monetizzazione della rete cloud è la larghezza di banda di rete in uscita. Ad esempio, trasferimenti di dati in uscita in Microsoft Azure modello aziendale. I dati in uscita vengono addebitati in base alla quantità totale di dati trasferiti dai Data Center di Azure tramite Internet in un determinato ciclo di fatturazione.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: lizross
author: eross-msft
ms.date: 10/02/2018
ms.openlocfilehash: 5425a562264addd3b2fc416f659f8ba79d6d99d6
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317493"
---
# <a name="egress-metering-in-a-virtual-network"></a>Misurazione in uscita in una rete virtuale

>Si applica a: Windows Server 2019


Un aspetto fondamentale della monetizzazione della rete cloud è la possibilità di fatturare l'utilizzo della larghezza di banda di rete. I dati in uscita vengono addebitati in base alla quantità totale di dati spostati fuori dalla data center tramite Internet in un determinato ciclo di fatturazione.

La misurazione in uscita per il traffico di rete SDN in Windows Server 2019 consente la possibilità di offrire contatori di utilizzo per i trasferimenti di dati in uscita. Il traffico di rete che lascia ogni rete virtuale, ma rimane entro il data center può essere rilevato separatamente, in modo da poter essere escluso dai calcoli di fatturazione. I pacchetti associati per gli indirizzi IP di destinazione che non sono inclusi in uno degli intervalli di indirizzi non fatturati vengono rilevati come trasferimenti di dati in uscita fatturati.

## <a name="virtual-network-unbilled-address-ranges-whitelist-of-ip-ranges"></a>Intervalli di indirizzi non fatturati della rete virtuale (whitelist degli intervalli IP)

È possibile trovare intervalli di indirizzi non fatturati nella proprietà **UnbilledAddressRanges** di una rete virtuale esistente. Per impostazione predefinita, non è stato aggiunto alcun intervallo di indirizzi.

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


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>Esempio: gestire gli intervalli di indirizzi non fatturati di una rete virtuale

È possibile gestire il set di prefissi di subnet IP da escludere dalla misurazione dell'uscita fatturata impostando la proprietà **UnbilledAddressRange** di una rete virtuale.  Qualsiasi traffico inviato dalle interfacce di rete nella rete virtuale con un indirizzo IP di destinazione corrispondente a uno dei prefissi non verrà incluso nella proprietà BilledEgressBytes.

1.  Aggiornare la proprietà **UnbilledAddressRanges** in modo che contenga le subnet che non verranno fatturate per l'accesso.

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >Se si aggiungono più subnet IP, utilizzare una virgola tra ognuna delle subnet IP.  Non includere spazi prima o dopo la virgola.

2.  Aggiornare la risorsa di rete virtuale con la proprietà **UnbilledAddressRanges** modificata.

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


         Tags             :
         ResourceRef      : /virtualNetworks/VNet1
         InstanceId       : 29654b0b-9091-4bed-ab01-e172225dc02d
         Etag             : W/"6970d0a3-3444-41d7-bbe4-36327968d853"
         ResourceMetadata :
         ResourceId       : VNet1
         Properties       : Microsoft.Windows.NetworkController.VirtualNetworkProperties
      ```


3. Controllare la rete virtuale per visualizzare il **UnbilledAddressRanges**configurato.

   ```PowerShell
   (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
   ```

   L'output sarà ora simile al seguente:
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

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>Controllare la fatturazione dell'utilizzo in uscita non fatturato di una rete virtuale

Dopo aver configurato la proprietà **UnbilledAddressRanges** , è possibile controllare l'utilizzo in uscita fatturato e non fatturato di ogni subnet all'interno di una rete virtuale. Il traffico in uscita viene aggiornato ogni quattro minuti con i byte totali degli intervalli fatturati e non fatturati.

Per ogni subnet virtuale sono disponibili le proprietà seguenti:

-   **UnbilledEgressBytes** Mostra il numero di byte non fatturati inviati dalle interfacce di rete connesse a questa subnet virtuale. I byte non fatturati sono byte inviati agli intervalli di indirizzi che fanno parte della proprietà **UnbilledAddressRanges** della rete virtuale padre.

-   **BilledEgressBytes** Mostra il numero di byte fatturati inviati dalle interfacce di rete connesse a questa subnet virtuale. I byte fatturati sono byte inviati agli intervalli di indirizzi che non fanno parte della proprietà **UnbilledAddressRanges** della rete virtuale padre.

Usare l'esempio seguente per eseguire una query sull'utilizzo in uscita:

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

L'output sarà simile al seguente:
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```


---
