---
title: Cmdlet per la configurazione dei dispositivi di memoria persistente per le macchine virtuali Hyper-V
description: Come configurare i dispositivi di memoria persistente per le macchine virtuali Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: jasgroce
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
author: coreyp-at-msft
ms.author: coreyp
ms.openlocfilehash: fd1b04ce74f0b8d490529d2a7f65091f5847d0f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878182"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>Cmdlet per la configurazione dei dispositivi di memoria persistente per le macchine virtuali Hyper-V

>Si applica a: Windows Server 2019

Questo articolo fornisce agli amministratori di sistema e i professionisti IT le informazioni sulla configurazione delle macchine virtuali Hyper-V con memoria persistente (noto anche come memoria della classe di archiviazione o memoria NVDIMM). I dispositivi di memoria persistente di memoria NVDIMM-N conformi JDEC sono supportati in Windows Server 2016 e Windows 10 e forniscono l'accesso a livello di byte per i dispositivi non volatile di latenza molto bassa. I dispositivi di memoria persistente di macchine Virtuali sono supportati in Windows Server 2019. 

## <a name="create-a-persistent-memory-device-for-a-vm"></a>Creare un dispositivo di memoria persistente per una macchina virtuale

Usare la **[New-VHD](https://docs.microsoft.com/powershell/module/hyper-v/new-vhd?view=win10-ps)** cmdlet per creare un dispositivo di memoria persistente per una macchina virtuale. In un volume NTFS DAX, è necessario creare il dispositivo.  La nuova estensione nome file (.vhdpmem) viene utilizzata per specificare che il dispositivo è un dispositivo di memoria persistente. È supportato solo il formato di file di disco rigido virtuale fisso.

**Esempio:** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>Creare una VM con un controller di memoria persistente



Usare la **cmdlet New-VM** per creare un Generation 2 VM con dimensioni di memoria specificato e il percorso di un'immagine VHDX. Quindi, usare **Add-VMPmemController** per aggiungere un controller di memoria persistente a una macchina virtuale.

**Esempio:** 
    
    New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

    Add-VMPmemController ProductionVM1x

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>Collegare un dispositivo di memoria persistente a una macchina virtuale

Uso **[Add-VMHardDiskDrive](https://docs.microsoft.com/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** collegare un dispositivo di memoria persistente a una macchina virtuale

**Esempio:** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

I dispositivi di memoria persistente all'interno di una macchina virtuale Hyper-V vengono visualizzati come un dispositivo di memoria persistente che verranno utilizzati e gestite dal sistema operativo guest. Sistemi operativi guest può usare il dispositivo come un blocco o un volume DAX. Quando i dispositivi di memoria persistente all'interno di una macchina virtuale vengono usati come un volume DAX, essi beneficiare a bassa latenza a livello di byte supporto degli indirizzi del dispositivo host (nessuna virtualizzazione i/o sul percorso del codice). 

>[!NOTE] 
>Memoria persistente è supportata solo per le macchine virtuali di Hyper-V Gen2. Live Migration e migrazione di archiviazione non sono supportati per le macchine virtuali con memoria persistente. I checkpoint di produzione di macchine virtuali non includono lo stato di memoria persistente. 