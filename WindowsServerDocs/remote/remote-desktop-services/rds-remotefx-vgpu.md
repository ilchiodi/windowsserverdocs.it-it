---
title: Servizi Desktop remoto - configurazione e installazione di GPU virtualizzata RemoteFX
description: Informazioni sulla pianificazione per configurare la virtualizzazione di grafica GPU virtualizzata RemoteFX.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3e7da1a70826dc720a96ceb3fe5d04868943f163
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876842"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>Installare e configurare GPU virtualizzata RemoteFX per Servizi Desktop remoto


La funzionalità di RemoteFX vGPU di rende possibile per più macchine virtuali condividere una scheda video grafiche fisiche. Le macchine virtuali sono in grado di eseguire l'offload di rendering delle informazioni grafiche dal processore alla scheda grafica dedicata. Per ridurre il carico della CPU e migliorare la scalabilità per grafici intensi carichi di lavoro che vengono eseguiti in macchine virtuali VDI. 

## <a name="remotefx-vgpu-requirements"></a>Requisiti di GPU virtualizzata RemoteFX

Requisiti per i sistemi host: 

- Windows Server 2016 o Windows 10
- 11.0 DX GPU compatibili con il driver compatibili WDDM 1.2 
- Ruolo Host di virtualizzazione Desktop remoto di Windows Server abilitato (Abilita il ruolo Hyper-V) 
- Server con una CPU che supporti SLAT (Second Level Address Translation) 

Requisiti delle macchine Virtuali Guest:

- VM guest che esegue un client di Windows Enterprise (Windows 7 con Service Pack 1, Windows 8.1, Windows 10) o Windows Server (Windows Server 2012 R2 o Windows Server 2016). Per altri sistemi operativi supportati vedere [Supported configuration for Remote Desktop Services](rds-supported-config.md).

Considerazioni aggiuntive per le macchine virtuali guest:

- OpenGL e OpenCL funzionalità è disponibile solo in Windows 10 o Windows Server 2016.  
- DirectX 11.0 è disponibile solo con Windows 8 o VM guest più recenti. 
- Host sessione Desktop remoto è supportato solo con GPU virtualizzata RemoteFX se viene eseguito come un [desktop di sessioni personali](rds-personal-session-desktops.md).

Per le macchine Virtuali guest, assicurarsi di esaminare [distribuzione VDI, sistemi operativi guest supportati](rds-supported-config.md#vdi-deployment--supported-guest-oss).

## <a name="install-remotefx-vgpu"></a>Installare GPU virtualizzata RemoteFX

Usare la procedura seguente per installare e configurare RemoteFX nell'host per Windows Server 2016 e Windows 10:

1. Installare il sistema operativo.
2. Installare il più recente Windows 10, Windows i driver GPU Server 2016 disponibili dal sito del fornitore scheda grafica.
3. Installare GPU virtualizzata RemoteFX nell'host Windows 10, Windows Server 2016:
   1. In un host di Windows 10, abilitare la funzionalità di Hyper-V nel Pannello di controllo (andare a pannello di controllo/programmi e funzionalità/Turn Windows funzionalità attiva o disattiva):

      ![Finestra di Windows per abilitare la funzionalità di Hyper-V](media/rds-hyperv-settings.png)

   2. In un host Windows Server 2016, installare il ruolo del Host di virtualizzazione Desktop remoto (RDVH).
   

4. A questo punto, creare e configurare una macchina virtuale guest:
   1. Creare una macchina virtuale con Windows 10 Enterprise o Windows Server 2016.
   2. Aggiungere la scheda grafica 3D di RemoteFX. Visualizzare [configurare la scheda 3D di RemoteFX vGPU](#configure-the-remotefx-vgpu-3d-adapter) per informazioni su come eseguire questa operazione con i cmdlet di PowerShell o gestione di Hyper-V. 

GPU virtualizzata RemoteFX verrà usati tutte le GPU sono disponibili più di uno. Tuttavia, in alcuni casi che si potrebbe voler limitare quali GPU vengono usate per RemoteFX. Nell'ambiente Hyper-V, si controlla questo comportamento selezionando in modo specifico che devono GPU *non* utilizzabile da RemoteFX. Eseguire i passaggi seguenti: 

   1. Passare alle impostazioni di Hyper-V nella console di gestione Hyper-V.
   2. Fare clic su **GPU fisiche** nelle impostazioni di Hyper-V.
   3. Selezionare la GPU che non si vuole usare e quindi deselezionare **usare la GPU con RemoteFX**.


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configurare la scheda RemoteFX vGPU 3D
È possibile usare i cmdlet di PowerShell o l'interfaccia utente di gestione di Hyper-V per configurare la scheda grafica 3D di RemoteFX vGPU. 

#### <a name="through-hyper-v-manager"></a>Tramite Gestione di Hyper-V:

1. Verificare che il sistema è stato impostato con Hyper-V e dispone di una macchina virtuale configurata.  
2. Arrestare la macchina virtuale, se è in esecuzione. 
3. In Hyper-V Manager passare al **impostazioni della macchina virtuale**, quindi fare clic su **aggiungere Hardware**.
4. Selezionare **scheda grafica 3D di RemoteFX**, fare clic su **Add**. 
5. Impostare il numero massimo di monitor e risoluzione monitor massima di memoria video dedicata o lasciare i valori predefiniti.

   > [!NOTE]
   > - Impostando i valori più alti per uno qualsiasi di queste opzioni avrà impatto per la scalabilità, pertanto è necessario impostare solo ciò che è assolutamente necessario.
   >
   > - Quando è necessario utilizzare 1GB di VRAM dedicata, usare una macchina virtuale guest a 64 bit anziché 32-bit (x86) per ottenere risultati ottimali.
6. Fare clic su **OK** per completare la configurazione.

#### <a name="with-powershell-cmdlets"></a>Con i cmdlet di PowerShell:

Eseguire i cmdlet seguenti per aggiungere, rivedere e configurare l'adattatore: 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Per informazioni dettagliate, vedere [Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter).

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

Per informazioni dettagliate vedere [Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter)

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Per informazioni dettagliate, vedere [Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter).

Eseguire il cmdlet seguente per esaminare le GPU fisiche:

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

Per informazioni dettagliate, vedere [Get-VMRemoteFXPhysicalVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter).

## <a name="monitor-performance"></a>Monitorare le prestazioni

Le prestazioni e scalabilità di un sistema VDI sono determinati da un'ampia gamma di fattori come la memoria totale della GPU, quantità di memoria di sistema e la velocità di memoria, numero di core CPU e la frequenza di clock della CPU, velocità di archiviazione e implementazione di NUMA.

Supporto per vGPU remote in Servizi Desktop remoto include i seguenti contatori delle prestazioni, che è possibile visualizzare in Performance Monitor (perfmon.exe) per raccogliere informazioni sulla velocità effettiva di frequenza fotogrammi.

- RemoteFX grafica - i contatori per la compressione della grafica di Remote Desktop Protocol. Ad esempio, se si desidera esaminare il numero dei frame viene presentato per la connessione RDP per la compressione, esaminare i **Frames/Second Input** contatore.
- Rete di RemoteFX - i contatori per il traffico di rete di Remote Desktop Protocol. Ad esempio, **tempo di Round Trip (RTT)**.
- Gestione di GPU RemoteFX Root - misure VRAM disponibili e riservate.
- Software di RemoteFX - fornisce contatori per la frequenza di acquisizione, il tempo di risposta GPU e altri.

Per un livello più basso il monitoraggio delle prestazioni, in particolare per la risoluzione dei problemi, è possibile usare i contatori delle prestazioni aggiuntive seguenti:

- Dispositivo delle macchine Virtuali VSC RemoteFX Synth3D 
- Canale di trasporto della macchina virtuale VSC RemoteFX Synth3D 
- VSP RemoteFX Synth3D 
- Dispositivo delle macchine Virtuali VSP RemoteFX Synth3D 
- Canale di macchina virtuale VSP RemoteFX Synth3D
