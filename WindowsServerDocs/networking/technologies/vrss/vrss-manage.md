---
title: Gestire vRSS
description: In questo argomento, si usano i comandi di Windows PowerShell per gestire vRSS in macchine virtuali (VM) e su host Hyper-V.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8af800608bee7037b48141a7a2edb0c872a7aac0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856192"
---
# <a name="manage-vrss"></a>Gestire vRSS

In questo argomento, è utilizzare i comandi di Windows PowerShell per gestire vRSS nelle macchine virtuali \(VMs\) e su Hyper\-host V.

>[!NOTE]
>Per altre informazioni sui comandi citati nel presente argomento, vedere [i comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

## <a name="vmq-on-hyper-v-hosts"></a>Coda macchine Virtuali negli host Hyper-V

Nell'host Hyper-V, è necessario usare le parole chiave che controllano i processori di coda macchine Virtuali.

**Visualizzare le impostazioni correnti:** 

```PowerShell
Get-NetAdapterVmq
```

**Configurare le impostazioni di coda macchine Virtuali:** 

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>le porte di commutazione vRSS in Hyper-V

Nell'host Hyper-V, è anche necessario abilitare vRSS sul Hyper\-porta del commutatore virtuale V.

**Visualizzare le impostazioni correnti:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Entrambe le impostazioni seguenti devono essere **True**. 

- VrssEnabledRequested: True
- VrssEnabled: True
    
>[!IMPORTANT]
>In alcune condizioni di limitazione risorse, una Hyper\-porta del commutatore virtuale V potrebbe non essere possibile a questa funzionalità è abilitata. Si tratta di una condizione temporanea e la funzionalità potrebbe diventare disponibile in un momento successivo.
>
>Se **VrssEnabled** viene **True**, quindi la funzionalità è abilitata per questa Hyper\-porta del commutatore virtuale V, vale a dire, per questa macchina virtuale o una scheda di rete virtuale.

**Configurare le impostazioni di vRSS porta commutatore:**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>vRSS in macchine virtuali e Vnic dell'host

È possibile usare gli stessi comandi usati per RSS nativa per configurare le impostazioni di vRSS in macchine virtuali e Vnic dell'host, che è anche il modo per abilitare RSS in Vnic dell'host.  

**Visualizzare le impostazioni correnti:**

```PowerShell
Get-NetAdapterRSS
```

**Configurare le impostazioni di vRSS:**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> Impostazione del profilo all'interno della VM non influisce sulla pianificazione del lavoro. Hyper\-V rende tutti la pianificazione di decisioni e ignora il profilo all'interno della VM.

## <a name="disable-vrss"></a>Disabilitare vRSS

È possibile disabilitare vRSS per disabilitare le impostazioni menzionate in precedenza.

- Disabilitare la funzionalità coda macchine Virtuali per l'interfaccia di rete fisico o macchina virtuale.

  >[!CAUTION]
  >La disabilitazione di coda macchine Virtuali on fisico interfaccia di rete influisce negativamente sulla capacità di Hyper\-host V per gestire i pacchetti in ingresso.

- Disabilitare vRSS per una macchina virtuale nel Hyper\-porte del commutatore virtuale V sul Hyper\-host V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Disabilitare vRSS per una scheda vNIC host sul Hyper\-porte del commutatore virtuale V sul Hyper\-host V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Disabilitare RSS nella macchina virtuale \(o vNIC host\) all'interno della VM \(o nell'host\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
