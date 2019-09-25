---
title: Abilitare l'hardware di monitoraggio delle prestazioni Intel in una macchina virtuale Hyper-V
description: Come abilitare l'hardware di monitoraggio delle prestazioni di Intel in un computer Hyper-V. Viene inoltre illustrato come abilitare la migrazione in tempo reale degli effetti hardware di monitoraggio delle prestazioni.
ms.prod: windows-server-threshold
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 67f32a6e2ceeaf07701d558f473e2f997fbd8219
ms.sourcegitcommit: d12d9e6afd71d23e8a24682ad80d2cf3bc486588
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2019
ms.locfileid: "71226021"
---
# <a name="enable-intel-performance-monitoring-hardware-in-a-hyper-v-virtual-machine"></a>Abilitare l'hardware di monitoraggio delle prestazioni Intel in una macchina virtuale Hyper-V

I processori Intel contengono funzionalità collettivamente denominate hardware di monitoraggio delle prestazioni, ad esempio PMU, PEBS, LBR. Queste funzionalità vengono usate dal software di ottimizzazione delle prestazioni, ad esempio Intel VTune Amplifier, per analizzare le prestazioni del software.  Prima di Windows Server 2019 e Windows 10 versione 1809, il sistema operativo host e le macchine virtuali guest Hyper-V non potevano utilizzare hardware di monitoraggio delle prestazioni quando Hyper-V è stato abilitato.  A partire da Windows Server 2019 e Windows 10 versione 1809, per impostazione predefinita il sistema operativo host può accedere all'hardware di monitoraggio delle prestazioni.  Le macchine virtuali guest Hyper-V non dispongono di accesso per impostazione predefinita, ma gli amministratori di Hyper-V possono scegliere di concedere l'accesso a una o più macchine virtuali guest.  In questo documento vengono descritti i passaggi necessari per esporre l'hardware di monitoraggio delle prestazioni alle macchine virtuali guest.

## <a name="requirements"></a>Requisiti

Per abilitare l'hardware di monitoraggio delle prestazioni in una macchina virtuale, è necessario:

- Processore Intel con hardware di monitoraggio delle prestazioni (ad esempio PMU, PEBS, IPT)
- Windows Server 2019 o Windows 10 versione 1809 (aggiornamento di ottobre 2018) o versione successiva
- Una macchina virtuale Hyper-V _senza_ [virtualizzazione annidata](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) anch ' essa nello stato interrotto
 
## <a name="enabling-performance-monitoring-components-in-a-virtual-machine"></a>Abilitazione dei componenti di monitoraggio delle prestazioni in una macchina virtuale

Per abilitare diversi componenti di monitoraggio delle prestazioni per una macchina virtuale Guest specifica, `Set-VMProcessor` usare il cmdlet di PowerShell:
 
``` Powershell
# Enable all components
Set-VMProcessor MyVMName -Perfmon @("pmu", "lbr", "pebs")
```
 
``` Powershell
# Enable a specific component
Set-VMProcessor MyVMName -Perfmon @("pmu")
```
 
``` Powershell
# Disable all components
Set-VMProcessor MyVMName -Perfmon @()
```
> [!NOTE]
> Quando si abilitano i componenti di monitoraggio `"pebs"` delle prestazioni, se `"pmu"` si specifica, è necessario specificare.  Inoltre, l'abilitazione di un componente non supportato dai processori fisici dell'host provocherà un errore di avvio della macchina virtuale.
 
## <a name="effects-of-enabling-performance-monitoring-hardware-on-saverestore-export-and-live-migration"></a>Effetti dell'abilitazione dell'hardware per il monitoraggio delle prestazioni in Salva/Ripristina, Esporta e migrazione in tempo reale
 
Microsoft non consiglia di eseguire la migrazione in tempo reale o di salvare/ripristinare le macchine virtuali con hardware di monitoraggio delle prestazioni tra sistemi con hardware Intel diverso. Il comportamento specifico dell'hardware di monitoraggio delle prestazioni è spesso non architettonico e cambia tra i sistemi hardware Intel.  Il trasferimento di una macchina virtuale in esecuzione tra sistemi diversi può causare un comportamento imprevedibile dei contatori non architetturali.