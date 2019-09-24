---
title: Esposizione dell'hardware di monitoraggio delle prestazioni Intel a una macchina virtuale Hyper-V
description: Viene descritto come esporre l'hardware Monitorning delle prestazioni di Intel a un computer Hyper-V. Vengono inoltre illustrate le modalità di migrazione in tempo reale degli effetti dell'abilitazione.
ms.prod: windows-server-threshold
ms.reviewer: ifufondu
author: ifeomaufondu-ms
ms.author: ifufondu
manager: chhuybre
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: 1bc821cc46a3a402778e1c76f4a2d8feb862fd75
ms.sourcegitcommit: 45415ba58907d650cfda45f4c57f6ddf1255dcbf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/24/2019
ms.locfileid: "71213617"
---
# <a name="exposing-intel-performance-monitoring-hardware-to-a-hyper-v-virtual-machine"></a>Esposizione dell'hardware di monitoraggio delle prestazioni Intel a una macchina virtuale Hyper-V
 
## <a name="background"></a>Sfondo
I processori Intel contengono funzionalità collettivamente denominate hardware di monitoraggio delle prestazioni, ad esempio PMU, PEBS, LBR. Queste funzionalità vengono usate dal software di ottimizzazione delle prestazioni, ad esempio Intel VTune Amplifier, per analizzare le prestazioni del software.  Prima di Windows Server 2019 e Windows 10 versione 1809, il sistema operativo host e le macchine virtuali guest Hyper-V non potevano utilizzare hardware di monitoraggio delle prestazioni quando Hyper-V è stato abilitato.  A partire da Windows Server 2019 e Windows 10 versione 1809, per impostazione predefinita il sistema operativo host può accedere all'hardware di monitoraggio delle prestazioni.  Le macchine virtuali guest Hyper-V non dispongono di accesso per impostazione predefinita, ma gli amministratori di Hyper-V possono scegliere di concedere l'accesso a una o più macchine virtuali guest.  In questo documento vengono descritti i passaggi necessari per esporre l'hardware di monitoraggio delle prestazioni alle macchine virtuali guest.
 
## <a name="requirements"></a>Requisiti 
Per esporre l'hardware di monitoraggio delle prestazioni in una macchina virtuale, è necessario:
- Processore Intel con hardware di monitoraggio delle prestazioni (ad esempio PMU, PEBS, IPT)
- Windows Server 2019 o Windows 10 versione 1809 (aggiornamento di ottobre 2018) o versione successiva
- Una macchina virtuale Hyper-V _senza_ [virtualizzazione annidata](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) in stato di arresto
 
## <a name="exposing-the-pmu-capabilities-pmu-lbr-pebs-to-virtual-machines-via-powershells-set-vmprocessor-cmdlet"></a>Esposizione delle funzionalità di PMU (PMU, LBR, PEBS) alle macchine virtuali tramite il cmdlet Set-VMProcessor di PowerShell
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
 
>Nota: Quando si abilitano i componenti PerfMon `"pebs"` , se si specifica `"pmu"` , è necessario specificare.  Inoltre, l'abilitazione di un componente PerfMon non supportato dai processori fisici dell'host provocherà un errore di avvio della macchina virtuale.
 
## <a name="effect-of-pmu-enabelement-on-saverestore-export-and-live-migration"></a>Effetto di PMU enabelement su Salva/Ripristina, Esporta e migrazione in tempo reale
 
Microsoft non consiglia di eseguire la migrazione in tempo reale o di salvare/ripristinare le macchine virtuali con hardware di monitoraggio delle prestazioni tra sistemi con hardware Intel diverso. Il comportamento specifico dell'hardware di monitoraggio delle prestazioni è spesso non architettonico e cambia tra i sistemi hardware Intel.  Il trasferimento di una macchina virtuale in esecuzione tra sistemi diversi può causare un comportamento imprevedibile dei contatori non architetturali.