---
title: 'Servizi Desktop remoto: configurazione e installazione di RemoteFX vGPU'
description: Informazioni sulla pianificazione per configurare la virtualizzazione della grafica con RemoteFX vGPU.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/23/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0263fa6b-2185-4cc3-99ef-3588e2f4ada5
author: lizap
manager: scottman
ms.openlocfilehash: a216b84383e6edb3e0537189af5938eb5d62ce42
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387270"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>Installare e configurare RemoteFX vGPU per Servizi Desktop remoto


La funzionalità vGPU di RemoteFX rende possibile la condivisione di una scheda video fisica tra più macchine virtuali. Le macchine virtuali possono eseguire l'offload del rendering delle informazioni grafiche dal processore alla scheda grafica dedicata. Questo ridurrà il carico della CPU e migliorerà la scalabilità per avere carichi di lavoro di grafica intensi eseguiti in macchine virtuali VDI. 

## <a name="remotefx-vgpu-requirements"></a>Requisiti di RemoteFX vGPU

Requisiti per i sistemi host: 

- Windows Server 2016 o Windows 10
- GPU compatibile con DX 11.0 e driver compatibile con WDDM 1.2 
- Ruolo Host di virtualizzazione Desktop remoto di Windows Server abilitato (abilita il ruolo Hyper-V) 
- Server con CPU che supporta SLAT (Second-Level Address Translation) 

Requisiti per le VM guest:

- VM guest che esegue un client di Windows Enterprise (Windows 7 con Service Pack 1, Windows 8.1, Windows 10) o Windows Server (Windows Server 2012 R2 o Windows Server 2016). Per altri sistemi operativi supportati vedere [Configurazione supportata per Servizi Desktop remoto](rds-supported-config.md).

Considerazioni aggiuntive per le VM guest:

- Le funzionalità OpenGL e OpenCL sono disponibili solo in Windows 10 o Windows Server 2016.  
- DirectX 11.0 è disponibile solo con Windows 8 o VM guest più recenti. 
- Host sessione Desktop remoto è supportato solo con RemoteFX vGPU se eseguito come un [desktop di sessioni personali](rds-personal-session-desktops.md).

Per le VM guest, assicurarsi di consultare [Distribuzione VDI: sistemi operativi guest supportati](rds-supported-config.md#vdi-deployment--supported-guest-oss).

## <a name="install-remotefx-vgpu"></a>Installare RemoteFX vGPU

Seguire questa procedura per installare e configurare RemoteFX sull'host per Windows Server 2016 e Windows 10:

1. Installare il sistema operativo.
2. Installare i driver della GPU di Windows 10/Windows Server 2016 più recenti, disponibili sul sito del fornitore della scheda grafica.
3. Installare RemoteFX vGPU sull'host di Windows 10/Windows Server 2016:
   1. Su un host di Windows 10, abilitare la funzionalità Hyper-V nel Pannello di controllo (andare su Pannello di controllo/Programmi e funzionalità/Attiva o disattiva funzionalità di Windows):

      ![Finestra delle funzionalità di Windows per abilitare la funzionalità Hyper-V](media/rds-hyperv-settings.png)

   2. Su un host di Windows Server 2016, installare il ruolo Host di virtualizzazione Desktop remoto (RDVH).
   

4. A questo punto, creare e configurare una VM guest:
   1. Creare una VM con Windows 10 Enterprise o Windows Server 2016.
   2. Aggiungere la scheda grafica 3D RemoteFX. Consultare [Configurare la scheda 3D RemoteFX vGPU](#configure-the-remotefx-vgpu-3d-adapter) per informazioni su come eseguire questa operazione con i comandi cmdlet di PowerShell o della Hyper-V Manager. 

Se è disponibile più di una GPU, RemoteFX vGPU le userà tutte. Tuttavia, in alcuni casi si potrebbe voler limitare quali GPU usare con RemoteFX. Nell'ambiente Hyper-V, questo comportamento si controlla specificando quali GPU *non* usare con RemoteFX. Eseguire i passaggi seguenti: 

   1. Passare alle impostazioni di Hyper-V nella Hyper-V Manager.
   2. Fare clic su **GPU fisiche** nelle impostazioni di Hyper-V.
   3. Seleziona la GPU che non si vuole usare e quindi deseleziona **Utilizza questa GPU con RemoteFX**.


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configurare la scheda 3D RemoteFX vGPU
È possibile usare i comandi cmdlet di PowerShell o dell'interfaccia utente della Hyper-V Manager per configurare la scheda grafica 3D RemoteFX vGPU. 

#### <a name="through-hyper-v-manager"></a>In Hyper-V Manager:

1. Assicurarsi che il sistema sia stato configurato con Hyper-V e che disponga di una VM configurata.  
2. Arrestare la VM, se è in esecuzione. 
3. Nella Hyper-V Manager passare a **Impostazioni VM** e quindi fare clic su **Aggiungi hardware**.
4. Selezionare **Scheda grafica 3D di RemoteFX** e fare clic su **Aggiungi**. 
5. Impostare il limite massimo di monitor, risoluzione del monitor e memoria video dedicata oppure lasciare i valori predefiniti.

   > [!NOTE]
   > - L'impostazione di valori più alti per una qualsiasi di queste opzioni avrà effetto sulla scalabilità, per cui è impostare solo quanto assolutamente necessario.
   >
   > - Quando è necessario usare 1GB di VRAM dedicata, usare una VM guest a 64 bit anziché 32 bit (x86) per avere risultati ottimali.
6. Fare clic su **OK** per salvare la configurazione.

#### <a name="with-powershell-cmdlets"></a>Comandi cmdlet di PowerShell:

Eseguire i comandi cmdlet seguenti per aggiungere, esaminare e configurare la scheda: 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Per informazioni dettagliate, consultare [Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter).

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

Per informazioni dettagliate, consultare [Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter)

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Per informazioni dettagliate, consultare [Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter).

Eseguire il comando cmdlet seguente per esaminare le GPU fisiche:

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

Per informazioni dettagliate, consultare [Get-VMRemoteFXPhysicalVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter).

## <a name="monitor-performance"></a>Monitorare le prestazioni

Le prestazioni e la scalabilità di un sistema VDI sono determinate da numerosi fattori, come memoria totale, quantità di memoria di sistema e velocità di memoria della GPU, numero di core CPU e frequenza di clock, velocità di archiviazione e implementazione NUMA della CPU.

Il supporto remoto per vGPU in Servizi Desktop remoto include i seguenti contatori delle prestazioni, che si possono visualizzare in Performance Monitor (perfmon.exe) per raccogliere informazioni sulla velocità effettiva della frequenza di fotogrammi.

- RemoteFX Graphics: contatori per la compressione della grafica di Remote Desktop Protocol. Ad esempio, se si desidera esaminare il numero dei frame presentato a Remote Desktop Protocol per la compressione, esaminare il contatore **Input Frames/Second**.
- RemoteFX Network: contatori per il traffico di rete di Remote Desktop Protocol. Ad esempio, **Round Trip Time (RTT)** .
- RemoteFX Root GPU Management: misura le VRAM disponibili e riservate.
- RemoteFX Software: offre contatori per la frequenza di acquisizione, il tempo di risposta della GPU e altro.

Per un monitoraggio delle prestazioni di livello più basso, in particolare per la risoluzione dei problemi, è possibile usare i seguenti contatori delle prestazioni aggiuntivi:

- RemoteFX Synth3D VSC VM Device 
- RemoteFX Synth3D VSC VM Transport Channel 
- RemoteFX Synth3D VSP 
- RemoteFX Synth3D VSP VM Device 
- RemoteFX Synth3D VSP VM Channel
