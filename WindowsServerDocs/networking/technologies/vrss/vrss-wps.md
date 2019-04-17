---
title: Comandi di Windows PowerShell per RSS e vRSS
description: In questo argomento, imparare a individuare rapidamente le informazioni di riferimento tecnico sui comandi di Windows PowerShell per ricevere lato ridimensionamento (RSS) e RSS virtuale (vRSS).
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
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339269"
---
# Comandi di Windows PowerShell per RSS e vRSS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento, imparare a individuare rapidamente le informazioni di riferimento tecnico sui comandi di Windows PowerShell per Receive Side Scaling \(RSS\) e \(vRSS\) RSS virtuale.

Utilizzare i seguenti comandi RSS per configurare RSS in un computer fisico con più processori o più core. È possibile utilizzare gli stessi comandi per configurare vRSS in una macchina virtuale \(VM\) che esegue un sistema operativo supportato. Per altre informazioni, vedi [Cmdlet degli adattatori di rete in Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps).

## Configurare VMQ

vRSS richiede che VMQ è abilitata e configurata. È possibile utilizzare i seguenti comandi di Windows PowerShell per gestire le impostazioni VMQ.

- [Disabilita NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/disable-netadaptervmq?view=win10-ps)
- [Enable-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/enable-netadaptervmq?view=win10-ps)
- [Get-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/get-netadaptervmq?view=win10-ps)
- [Set-NetAdapterVmq](https://docs.microsoft.com/powershell/module/netadapter/set-netadaptervmq?view=win10-ps)

## Abilitare e configurare RSS in un host nativo

Utilizzare i seguenti comandi di PowerShell per configurare RSS in un host nativo, nonché gestire RSS in una macchina virtuale o in un host NIC virtuale (vNIC). Alcuni dei parametri di questi comandi anche potrebbe influire sulla macchina virtuale coda \(VMQ\) nell'host Hyper-V.  

>[!IMPORTANT]
>L'abilitazione RSS in una macchina virtuale o in una scheda vNIC host è un prerequisito per l'attivazione e uso vRSS.

- [Disabilita NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/disable-netadapterrss?view=win10-ps)
- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/enable-netadapterrss?view=win10-ps)
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/get-netadapterrss?view=win10-ps)
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss?view=win10-ps)

## Abilitare vRSS sulla porta commutatore virtuale Hyper-V

Oltre ad abilitare RSS nella macchina virtuale, vRSS richiede che puoi abilitare vRSS sulla porta commutatore virtuale Hyper-V. 

Determinare le impostazioni presenti per vRSS e abilitare o disabilitare la funzionalità per una macchina virtuale.

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | fl
   ```

   **Abilitato la funzionalità:**
   
   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssEnabled [$True|$False]
   ```

## Abilitare o disabilitare vRSS su una scheda vNIC host

Determinare le impostazioni presenti per vRSS e abilitare o disabilitare la funzionalità per una scheda vNIC host.

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | fl
   ```

   **Abilitare o disabilitare la funzionalità:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled [$True|$False]
   ```

## Configurare la modalità di programmazione sulla porta commutatore virtuale Hyper-V 
>Si applica a: Windows Server 2019

In Windows Server 2019, vRSS aggiornare i processori logici utilizzati per elaborare il traffico di rete in modo dinamico.  I dispositivi con driver supportati avere questa modalità di programmazione abilitata per impostazione predefinita. 

Determinare la modalità di programmazione presente in un sistema o modificare la modalità di pianificazione di una macchina virtuale.

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter <vm-name> | Select 'VRSSQueue'
   ```

   **Impostare o modificare la modalità di programmazione:**

   ```PowerShell
   Set-VMNetworkAdapter <vm-name> -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```

## Configurare la modalità di programmazione in una scheda vNIC host
>Si applica a: Windows Server 2019

Per determinare la modalità di programmazione presente o per modificare la modalità di pianificazione di una scheda vNIC host, Usa i comandi di Windows PowerShell seguenti:

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS | Select 'VRSSQueue'
   ```

   **Impostare o modificare la modalità di programmazione:** 

   ```PowerShell
   Set-VMNetworkAdapter -ManagementOS -VrssQueueSchedulingMode -VrssQueueSchedulingMode [Dynamic|$StaticVrss|StaticVMQ]
   ```


## Argomenti correlati 
Per altre informazioni, Vedi gli argomenti di riferimento seguenti.

- [Get-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmnetworkadapter)
- [Set-VMNetworkAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmnetworkadapter)

Per altre informazioni, vedi [Virtual Receive Side Scaling (vRSS)](vrss-top.md).