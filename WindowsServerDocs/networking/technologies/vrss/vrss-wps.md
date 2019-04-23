---
title: Comandi di Windows PowerShell per RSS e vRSS
description: In questo argomento descrive come individuare rapidamente le informazioni di riferimento tecnico sui comandi di Windows PowerShell per Receive-Side Scaling (RSS) e RSS virtuale (vRSS).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 49e93b9f-46d9-4cee-bcda-1c4634893ddd
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/05/2018
ms.openlocfilehash: 10039388009e32c10d71067b835bad65db5607ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833262"
---
# <a name="windows-powershell-commands-for-rss-and-vrss"></a>Comandi di Windows PowerShell per RSS e vRSS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento descrive come individuare rapidamente le informazioni di riferimento tecnico sui comandi di Windows PowerShell per Receive-Side Scaling \(RSS\) e virtuale RSS \(vRSS\).

Usare i comandi seguenti di RSS configurare RSS in un computer fisico con più processori o multicore. È possibile usare gli stessi comandi per configurare vRSS in una macchina virtuale \(VM\) che esegue un sistema operativo supportato. Per altre informazioni, vedere [cmdlet per scheda di rete in Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## <a name="configure-vmq"></a>Configurare funzionalità coda macchine Virtuali

vRSS richiede che coda macchine Virtuali sia abilitato e configurato. È possibile usare i comandi di Windows PowerShell seguenti per gestire le impostazioni di coda macchine Virtuali.

- [Disable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## <a name="enable-and-configure-rss-on-a-native-host"></a>Abilitare e configurare RSS in un host nativo

Usare i comandi PowerShell seguenti per configurare RSS in un host nativo, nonché gestire RSS in una macchina virtuale o in un host NIC virtuale (vNIC). Alcuni dei parametri di questi comandi dipende anche dalla coda macchine virtuali \(VMQ\) nell'host Hyper-V.  

>[!IMPORTANT]
>L'abilitazione di RSS in una macchina virtuale o in una scheda vNIC host è un prerequisito per l'abilitazione e uso di vRSS.

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## <a name="enable-vrss-on-the-hyper-v-virtual-switch-port"></a>Abilitare vRSS sul Hyper\-porta del commutatore virtuale V

Oltre ad abilitare RSS nella macchina virtuale, è necessario abilitare vRSS sull'Hyper vRSS\-porta del commutatore virtuale V. 

Determinare le impostazioni presente per vRSS e abilitare o disabilitare la funzionalità per una macchina virtuale.

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **È stata abilitata la funzionalità:**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## <a name="enable-or-disable-vrss-on-a-host-vnic"></a>Abilitare o disabilitare vRSS su una scheda vNIC host

Determinare le impostazioni correnti di vRSS e abilitare o disabilitare la funzionalità per una scheda vNIC host.

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Abilitare o disabilitare la funzionalità:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## <a name="configure-the-scheduling-mode-on-the-hyper-v-virtual-switch-port"></a>Configurare la modalità di pianificazione nella porta del commutatore virtuale Hyper-V 
>Si applica a: Windows Server 2019

In Windows Server 2019, vRSS possibile aggiornare i processori logici usati per elaborare il traffico di rete in modo dinamico.  I dispositivi con i driver supportati hanno questa modalità pianificazione abilitata per impostazione predefinita. 

Determinare la modalità di pianificazione presenta in un sistema o modificare la modalità di pianificazione per una macchina virtuale.

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Impostare o modificare la modalità di pianificazione:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## <a name="configure-the-scheduling-mode-on-a-host-vnic"></a>Configurare la modalità di pianificazione in una scheda vNIC host
>Si applica a: Windows Server 2019

Per determinare la modalità pianificazione attuale o per modificare la modalità di pianificazione per una scheda vNIC host, usare i comandi di Windows PowerShell seguenti:

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Impostare o modificare la modalità di pianificazione:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## <a name="related-topics"></a>Argomenti correlati 
Per altre informazioni, vedere gli argomenti di riferimento seguenti.

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Per altre informazioni, vedere [Virtual Receive-Side Scaling (vRSS)](vrss-top.md).