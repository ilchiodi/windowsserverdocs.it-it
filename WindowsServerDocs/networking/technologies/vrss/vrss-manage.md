---
title: Gestire vRSS
description: In questo argomento vengono usati i comandi di Windows PowerShell per gestire vRSS in macchine virtuali (VM) e in host Hyper-V.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0fe5bfc3-591f-4a19-b98a-0668d4c9f93a
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9d528f7e658d61f613eedc635fb81d8f18fd59aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405163"
---
# <a name="manage-vrss"></a>Gestire vRSS

In questo argomento vengono usati i comandi di Windows PowerShell per gestire vRSS in macchine virtuali \(VM\) e negli host Hyper\-V.

>[!NOTE]
>Per ulteriori informazioni sui comandi descritti in questo argomento, vedere [comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

## <a name="vmq-on-hyper-v-hosts"></a>VMQ negli host Hyper-V

Nell'host Hyper-V è necessario utilizzare le parole chiave che controllano i processori VMQ.

**Visualizzare le impostazioni correnti:** 

```PowerShell
Get-NetAdapterVmq
```

**Configurare le impostazioni di VMQ:** 

```PowerShell
Set-NetAdapterVmq
```


## <a name="vrss-on-hyper-v-switch-ports"></a>vRSS sulle porte del Commuter Hyper-V

Nell'host Hyper-V è necessario abilitare anche vRSS sulla porta del Commuter virtuale Hyper\-V.

**Visualizzare le impostazioni correnti:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Entrambe le impostazioni seguenti devono essere **true**. 

- VrssEnabledRequested: true
- VrssEnabled: true
    
>[!IMPORTANT]
>In alcune condizioni di limitazione delle risorse, una porta del Commuter virtuale Hyper\-V potrebbe non essere in grado di abilitare questa funzionalità. Si tratta di una condizione temporanea e la funzionalità può diventare disponibile in un momento successivo.
>
>Se **VrssEnabled** è **true**, la funzionalità è abilitata per la porta del Commuter virtuale Hyper\-V, ovvero per questa macchina virtuale o vNIC.

**Configurare le impostazioni della porta di commutazione vRSS:**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## <a name="vrss-in-vms-and-host-vnics"></a>vRSS in VM e host schede

È possibile usare gli stessi comandi usati per i feed RSS nativi per configurare le impostazioni vRSS nelle VM e nell'host schede, che è anche la modalità di abilitazione di RSS nell'host schede.  

**Visualizzare le impostazioni correnti:**

```PowerShell
Get-NetAdapterRSS
```

**Configurare le impostazioni di vRSS:**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> L'impostazione del profilo all'interno della macchina virtuale non influisca sulla pianificazione del lavoro. Hyper\-V prende tutte le decisioni di pianificazione e ignora il profilo all'interno della macchina virtuale.

## <a name="disable-vrss"></a>Disabilitare vRSS

È possibile disabilitare vRSS per disabilitare le impostazioni indicate in precedenza.

- Disabilitare VMQ per la scheda di interfaccia di rete fisica o la macchina virtuale.

  >[!CAUTION]
  >La disabilitazione di VMQ sulla scheda di interfaccia di rete fisica influisca gravemente sulla capacità dell'host Hyper\-V di gestire i pacchetti in ingresso.

- Disabilitare vRSS per una macchina virtuale nella porta del commutire virtuale Hyper\-V nell'host Hyper\-V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Disabilitare vRSS per un host vNIC sulla porta del Commuter virtuale Hyper\-V nell'host Hyper\-V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Disabilitare RSS nella macchina virtuale \(o host vNIC\) all'interno della VM \(o nell'host\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
