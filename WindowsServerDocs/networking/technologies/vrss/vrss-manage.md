---
title: Gestire vRSS
description: In questo argomento, usare i comandi di Windows PowerShell per gestire vRSS in macchine virtuali (VM) e negli host Hyper-V.
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133837"
---
# Gestire vRSS

In questo argomento, usare i comandi di Windows PowerShell per gestire vRSS in macchine virtuali \(VMs\) e negli host Hyper-V.

>[!NOTE]
>Per altre informazioni sui comandi citati in questo argomento, vedi [I comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

## VMQ su host Hyper-V

Nell'host Hyper-V, è necessario utilizzare le parole chiave che controllano i processori VMQ.

**Visualizzare le impostazioni correnti:** 

```PowerShell
Get-NetAdapterVmq
```

**Configurare le impostazioni di VMQ:** 

```PowerShell
Set-NetAdapterVmq
```


## porte di commutazione vRSS in Hyper-V

Nell'host Hyper-V, devi anche abilitare vRSS sulla porta commutatore virtuale Hyper-V.

**Visualizzare le impostazioni correnti:**

```PowerShell
Get-VMNetworkAdapter <vm-name> | fl

Get-VMNetworkAdapter -ManagementOS | fl
```
    
Entrambe le impostazioni seguenti devono essere **True**. 

- VrssEnabledRequested: True
- VrssEnabled: True
    
>[!IMPORTANT]
>In alcune condizioni di limitazione delle risorse, una porta di commutatore virtuale Hyper-V potrebbe essere in grado di abilitare questa funzionalità. Si tratta di una condizione temporanea e la funzionalità può diventare disponibile in un momento successivo.
>
>Se **VrssEnabled** è **impostato su True**, quindi la funzionalità è abilitata per questa porta commutatore virtuale Hyper-V, vale a dire per questa macchina virtuale o vNIC.

**Configurare le impostazioni di vRSS porta switch:**

```PowerShell
Set-VMNetworkAdapter <vm-name> -VrssEnabled $TRUE
    
Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
```

## vRSS in macchine virtuali e vnic dell'host

È possibile utilizzare gli stessi comandi utilizzati per RSS nativo per configurare le impostazioni di vRSS in macchine virtuali e vnic dell'host, che è anche il modo per attivare RSS vnic dell'host.  

**Visualizzare le impostazioni correnti:**

```PowerShell
Get-NetAdapterRSS
```

**Configurare le impostazioni di vRSS:**

```PowerShell
Set-NetAdapterRss
```

>[!NOTE]
> Impostazione del profilo all'interno della macchina virtuale non influisce la programmazione delle operazioni. Hyper-V rende tutte le decisioni di pianificazione e ignora il profilo all'interno della macchina virtuale.

## Disabilita vRSS

Puoi disabilitare vRSS per disabilitare le impostazioni accennate in precedenza.

- Disabilitare VMQ per la scheda di rete fisica o la macchina virtuale.

  >[!CAUTION]
  >La disabilitazione di VMQ sulla fisica NIC influisce negativamente sulla possibilità di host Hyper-V per gestire i pacchetti in ingresso.

- Disabilitare vRSS per una macchina virtuale sulla porta commutatore virtuale Hyper-V nell'host Hyper-V.

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled $FALSE
   ```

- Disabilita vRSS per una scheda vNIC host sulla porta commutatore virtuale Hyper-V nell'host Hyper-V.

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $FALSE
   ```

- Disabilitare RSS nella macchina virtuale \(or host vNIC\) all'interno della macchina virtuale \ (o il host\)

   ```PowerShell
   Disable-NetAdapterRSS *
   ```
