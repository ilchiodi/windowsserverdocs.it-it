---
title: Misurazione in uscita nella rete virtuale
description: 'Un aspetto fondamentale di monetizzazione rete cloud è in uscita della larghezza di banda di rete. Ad esempio: dati in uscita trasferisce il modello di business In Microsoft Azure. Dati in uscita viene addebitati in base alla quantità totale di dati spostati esternamente Data Center di Azure tramite Internet in un determinato ciclo di fatturazione.'
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: ad1bed11308420e271b8e06410d5a4548181314a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876422"
---
# <a name="egress-metering-in-a-virtual-network"></a>Misurazione in uscita in una rete virtuale

>Si applica a: Windows Server 2019


Un aspetto fondamentale di monetizzazione rete cloud è in grado di fatturare dall'utilizzo della larghezza di banda di rete. Dati in uscita viene addebitati in base alla quantità totale di dati spostati esternamente il data center tramite Internet in un determinato ciclo di fatturazione.

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
    
    
    Tags             :
    ResourceRef      : /virtualNetworks/VNet1
    InstanceId       : 29654b0b-9091-4bed-ab01-e172225dc02d
    Etag             : W/"6970d0a3-3444-41d7-bbe4-36327968d853"
    ResourceMetadata :
    ResourceId       : VNet1
    Properties       : Microsoft.Windows.NetworkController.VirtualNetworkProperties
    ```


3.  Controllare la rete virtuale per visualizzare l'applicazione configurata **UnbilledAddressRanges**.

    ```PowerShell
    (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
    ```

    L'output avrà un aspetto simile al seguente:
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

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>Controllare il fatturato l'utilizzo in uscita resoconti di una rete virtuale

Dopo aver configurato il **UnbilledAddressRanges** proprietà, è possibile controllare l'utilizzo in uscita fatturati e resoconti di ogni subnet all'interno di una rete virtuale. Il traffico in uscita Aggiorna ogni quattro minuti con i byte totali degli intervalli di fatturato e resoconti.

Le proprietà seguenti sono disponibili per ogni subnet virtuale:

-   **UnbilledEgressBytes** Mostra il numero di byte non fatturati inviati da interfacce di rete connesse a questa subnet virtuale. Byte resoconti vengono inviati a intervalli di indirizzi che fanno parte di byte i **UnbilledAddressRanges** proprietà della rete virtuale padre.

-   **BilledEgressBytes** Mostra numero di byte fatturati inviati da interfacce di rete connesse a questa subnet virtuale. Fatturato byte sono byte inviati a intervalli di indirizzi che non sono in parte i **UnbilledAddressRanges** proprietà della rete virtuale padre.

Utilizzare il seguente esempio di utilizzo in uscita di query:

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