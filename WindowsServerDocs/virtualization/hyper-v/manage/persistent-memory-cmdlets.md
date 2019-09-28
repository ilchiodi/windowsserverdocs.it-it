---
title: Cmdlet per la configurazione di dispositivi di memoria permanenti per macchine virtuali Hyper-V
description: Come configurare dispositivi di memoria permanenti per macchine virtuali Hyper-V
ms.prod: windows-server
ms.service: na
manager: jasgroce
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc08039ba1d
author: coreyp-at-msft
ms.author: coreyp
ms.openlocfilehash: ecae1fe96bc5088fa840c6e2e24a75bb72a9e8f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392536"
---
# <a name="cmdlets-for-configuring-persistent-memory-devices-for-hyper-v-vms"></a>Cmdlet per la configurazione di dispositivi di memoria permanenti per macchine virtuali Hyper-V

>Si applica a: Windows Server 2019

Questo articolo fornisce agli amministratori di sistema e ai professionisti IT informazioni sulla configurazione di macchine virtuali Hyper-V con memoria persistente (noto come memoria della classe di archiviazione o NVDIMM). I dispositivi di memoria permanenti NVDIMM-N compatibili con JDEC sono supportati in Windows Server 2016 e Windows 10 e offrono l'accesso a livello di byte a dispositivi non volatili a bassa latenza. I dispositivi di memoria permanenti VM sono supportati in Windows Server 2019. 

## <a name="create-a-persistent-memory-device-for-a-vm"></a>Creare un dispositivo di memoria permanente per una macchina virtuale

Usare il cmdlet **[New-VHD](https://docs.microsoft.com/powershell/module/hyper-v/new-vhd?view=win10-ps)** per creare un dispositivo di memoria permanente per una macchina virtuale. Il dispositivo deve essere creato in un volume DAX NTFS esistente.  La nuova estensione di file (con estensione vhdpmem) viene usata per specificare che il dispositivo è un dispositivo di memoria permanente. È supportato solo il formato di file VHD fisso.

**Esempio:** `New-VHD d:\VMPMEMDevice1.vhdpmem -Fixed -SizeBytes 4GB`

## <a name="create-a-vm-with-a-persistent-memory-controller"></a>Creare una VM con un controller di memoria permanente



Usare il **cmdlet New-VM** per creare una macchina virtuale di generazione 2 con la dimensione di memoria specificata e il percorso di un'immagine di VHDX. Quindi, usare **Add-VMPmemController** per aggiungere un controller di memoria permanente a una macchina virtuale.

**Esempio:** 
    
    New-VM -Name "ProductionVM1" -MemoryStartupBytes 1GB -VHDPath c:\vhd\BaseImage.vhdx

    Add-VMPmemController ProductionVM1x

## <a name="attach-a-persistent-memory-device-to-a-vm"></a>Connessione di un dispositivo di memoria permanente a una macchina virtuale

Usare **[Add-VMHardDiskDrive](https://docs.microsoft.com/powershell/module/hyper-v/add-vmharddiskdrive?view=win10-ps)** per alleghi un dispositivo di memoria permanente a una macchina virtuale

**Esempio:** `Add-VMHardDiskDrive ProductionVM1 PMEM -ControllerLocation 1 -Path D:\VPMEMDevice1.vhdpmem`

I dispositivi di memoria permanenti all'interno di una macchina virtuale Hyper-V vengono visualizzati come un dispositivo di memoria permanente che deve essere utilizzato e gestito dal sistema operativo guest. I sistemi operativi guest possono usare il dispositivo come un blocco o un volume DAX. Quando i dispositivi di memoria permanenti all'interno di una macchina virtuale vengono usati come volume DAX, traggono vantaggio dalla capacità di indirizzo a livello di byte a bassa latenza del dispositivo host (nessuna virtualizzazione I/O nel percorso del codice). 

>[!NOTE] 
>La memoria persistente è supportata solo per le macchine virtuali Gen2 Hyper-V. La migrazione di Live Migration e archiviazione non è supportata per le macchine virtuali con memoria permanente. I checkpoint di produzione delle macchine virtuali non includono lo stato della memoria persistente. 