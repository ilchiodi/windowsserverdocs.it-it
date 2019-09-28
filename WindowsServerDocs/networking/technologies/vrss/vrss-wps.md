---
title: Comandi di Windows PowerShell per RSS e vRSS
description: In questo argomento viene illustrato come individuare rapidamente informazioni di riferimento tecnico sui comandi di Windows PowerShell per Receive-Side Scaling (RSS) e RSS virtuale (vRSS).
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/05/2018
ms.openlocfilehash: bb915f72e53d28c73a9c2e405b3b0edd656953db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405281"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>Comandi di Windows PowerShell per RSS e vRSS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento si apprenderà come individuare rapidamente le informazioni di riferimento tecnico sui comandi di Windows PowerShell per Receive-Side Scaling \(RSS @ no__t-1 e Virtual RSS \(vRSS @ no__t-3.

Usare i comandi RSS seguenti per configurare RSS in un computer fisico con più processori o più core. È possibile usare gli stessi comandi per configurare vRSS in una macchina virtuale \(VM @ no__t-1 che esegue un sistema operativo supportato. Per altre informazioni, vedere [cmdlet per le schede di rete in Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## <a name="configure-vmq"></a>Configurare VMQ

vRSS richiede che VMQ sia abilitato e configurato. Per gestire le impostazioni VMQ, è possibile usare i comandi di Windows PowerShell seguenti.

- [Disable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## <a name="enable-and-configure-rss-on-a-native-host"></a>Abilitare e configurare RSS in un host nativo

Usare i comandi di PowerShell seguenti per configurare RSS in un host nativo, nonché per gestire RSS in una VM o in una scheda di interfaccia di rete virtuale host (vNIC). Alcuni parametri di questi comandi potrebbero influire anche Coda macchine virtuali \(VMQ @ no__t-1 nell'host Hyper-V.  

>[!IMPORTANT]
>L'abilitazione di RSS in una macchina virtuale o in un host vNIC è un prerequisito per l'abilitazione e l'uso di vRSS.

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>Abilitare vRSS sulla porta del Commuter virtuale Hyper @ no__t-0V

Oltre ad abilitare RSS nella macchina virtuale, vRSS richiede l'abilitazione di vRSS sulla porta del Commuter virtuale Hyper @ no__t-0V. 

Determinare le impostazioni presenti per vRSS e abilitare o disabilitare la funzionalità per una macchina virtuale.

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **Abilitazione della funzionalità:**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>Abilitare o disabilitare vRSS in un host vNIC

Determinare le impostazioni presenti per vRSS e abilitare o disabilitare la funzionalità per un vNIC host.

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Abilitare o disabilitare la funzionalità:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>Configurare la modalità di pianificazione sulla porta del Commuter virtuale Hyper-V 
>Si applica a: Windows Server 2019

In Windows Server 2019, vRSS è in grado di aggiornare in modo dinamico i processori logici utilizzati per elaborare il traffico di rete.  I dispositivi con driver supportati hanno questa modalità di pianificazione abilitata per impostazione predefinita. 

Determinare la modalità di pianificazione corrente in un sistema o modificare la modalità di pianificazione per una macchina virtuale.

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Impostare o modificare la modalità di pianificazione:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>Configurare la modalità di pianificazione in un host vNIC
>Si applica a: Windows Server 2019

Per determinare la modalità di pianificazione attuale o per modificare la modalità di pianificazione per un vNIC host, usare i comandi seguenti di Windows PowerShell:

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Impostare o modificare la modalità di pianificazione:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## <a name="related-topics"></a>Argomenti correlati 
Per ulteriori informazioni, vedere gli argomenti di riferimento seguenti.

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Per ulteriori informazioni, vedere [Virtual Receive Side Scaling (vRSS)](vrss-top.md).