---
title: Distribuire dispositivi grafici con RemoteFX vGPU
description: Informazioni su come distribuire e configurare RemoteFX vGPU in Windows Server
ms.prod: windows-server-threshold
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 08/21/2019
ms.openlocfilehash: 7111899557279d825191948e737d83d7467ce786
ms.sourcegitcommit: 81198fbf9e46830b7f77dcd345b02abb71ae0ac2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2019
ms.locfileid: "72923913"
---
# <a name="deploy-graphics-devices-using-remotefx-vgpu"></a>Distribuire dispositivi grafici con RemoteFX vGPU

> Si applica a: Windows Server 2016, Microsoft Hyper-V Server 2016

La funzionalità vGPU per RemoteFX rende possibile la condivisione di una GPU fisica da parte di più macchine virtuali. Le risorse di calcolo e di rendering vengono condivise in modo dinamico tra le macchine virtuali, rendendo RemoteFX vGPU appropriato per i carichi di lavoro con picchi elevati in cui non sono necessarie risorse GPU dedicate. Ad esempio, in un servizio VDI, RemoteFX vGPU può essere usato per eseguire l'offload dei costi di rendering delle app nella GPU, con l'effetto di ridurre il carico della CPU e migliorare la scalabilità del servizio.

## <a name="remotefx-vgpu-requirements"></a>Requisiti di RemoteFX vGPU

Requisiti di sistema dell'host:

- Windows Server 2016
- Una GPU compatibile con DirectX 11,0 con un driver compatibile con WDDM 1,2
- CPU con supporto per la conversione degli indirizzi di secondo livello

Requisiti per le VM guest:

- Sistema operativo guest supportato. Per ulteriori informazioni, vedere [supporto della scheda video RemoteFX 3D (vGPU)](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support).

Considerazioni aggiuntive per le VM guest:

- La funzionalità OpenGL e OpenCL è disponibile solo nei guest che eseguono Windows 10 o Windows Server 2016.  
- DirectX 11,0 è disponibile solo per i guest che eseguono Windows 8 o versioni successive.

## <a name="enable-remotefx-vgpu"></a>Abilita vGPU RemoteFX

Per configurare RemoteFX vGPU nell'host Windows Server 2016:

1. Installare i driver grafici consigliati dal fornitore della GPU per Windows Server 2016.
2. Creare una macchina virtuale che esegue un sistema operativo guest supportato da RemoteFX vGPU. Per altre informazioni, vedere [supporto per la scheda video 3D RemoteFX (vGPU)](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support).
3. Aggiungere la scheda grafica 3D RemoteFX alla macchina virtuale. Per altre informazioni, vedere [Configure the RemoteFX VGPU 3D Adapter](#configure-the-remotefx-vgpu-3d-adapter).

Per impostazione predefinita, RemoteFX vGPU userà tutte le GPU disponibili e supportate. Per limitare le GPU utilizzate da RemoteFX vGPU, attenersi alla procedura seguente:

1. Passare alle impostazioni di Hyper-V nella Hyper-V Manager.
2. Selezionare **GPU fisiche** in Impostazioni Hyper-V.
3. Seleziona la GPU che non si vuole usare e quindi deseleziona **Utilizza questa GPU con RemoteFX**.

### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configurare la scheda 3D RemoteFX vGPU

È possibile usare i comandi cmdlet di PowerShell o dell'interfaccia utente della Hyper-V Manager per configurare la scheda grafica 3D RemoteFX vGPU.

#### <a name="configure-remotefx-vgpu-with-hyper-v-manager"></a>Configurare RemoteFX vGPU con la console di gestione di Hyper-V

1. Arrestare la macchina virtuale se è attualmente in esecuzione.
2. Aprire la console di gestione di Hyper-V, passare a **Impostazioni macchina virtuale**e quindi selezionare **Aggiungi hardware**.
3. Selezionare **scheda grafica 3D RemoteFX**, quindi selezionare **Aggiungi**.
4. Impostare il limite massimo di monitor, risoluzione del monitor e memoria video dedicata oppure lasciare i valori predefiniti.

   > [!NOTE]
   > - L'impostazione di valori più elevati per una di queste opzioni influirà sulla scalabilità del servizio, pertanto è necessario impostare solo gli elementi necessari.
   > - Quando è necessario usare 1 GB di VRAM dedicata, usare una macchina virtuale Guest a 64 bit anziché 32 bit (x86) per ottenere risultati ottimali.

5. Selezionare **OK** per terminare la configurazione.

#### <a name="configure-remotefx-vgpu-with-powershell-cmdlets"></a>Configurare RemoteFX vGPU con i cmdlet di PowerShell

Usare i cmdlet di PowerShell seguenti per aggiungere, rivedere e configurare l'adapter:

- [Add-VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/add-vmremotefx3dvideoadapter?view=win10-ps)
- [Get-VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/get-vmremotefx3dvideoadapter?view=win10-ps)
- [Set-VMRemoteFx3dVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/set-vmremotefx3dvideoadapter?view=win10-ps)
- [Get-VMRemoteFXPhysicalVideoAdapter](https://docs.microsoft.com/powershell/module/hyper-v/get-vmremotefxphysicalvideoadapter?view=win10-ps)

## <a name="monitor-performance"></a>Monitorare le prestazioni

Le prestazioni e la scalabilità di un servizio abilitato per vGPU RemoteFX sono determinate da diversi fattori, ad esempio il numero di GPU nel sistema, la memoria GPU totale, la quantità di memoria di sistema e la velocità della memoria, il numero di core CPU e la frequenza di clock della CPU, la velocità di archiviazione e NUMA implementazione.

### <a name="host-system-memory"></a>Memoria di sistema host

Per ogni macchina virtuale abilitata con vGPU, RemoteFX usa memoria di sistema sia nel sistema operativo guest che nel server host. L'hypervisor garantisce la disponibilità della memoria di sistema per un sistema operativo guest. Nell'host ogni desktop virtuale abilitato per vGPU deve annunciare il requisito relativo alla memoria di sistema per l'hypervisor. Quando viene avviato il desktop virtuale abilitato per vGPU, l'hypervisor riserva memoria di sistema aggiuntiva nell'host.

Il requisito di memoria per il server abilitato per RemoteFX è dinamico perché la quantità di memoria utilizzata nel server abilitato per RemoteFX dipende dal numero di monitoraggi associati ai desktop virtuali abilitati per vGPU e dalla risoluzione massima per questi monitoraggi.

### <a name="host-gpu-video-memory"></a>Memoria video GPU host

Ogni desktop virtuale abilitato per vGPU usa la memoria video hardware GPU nel server host per eseguire il rendering del desktop. Inoltre, un codec usa la memoria video per comprimere la schermata di cui è stato eseguito il rendering. La quantità di memoria necessaria per il rendering e la compressione è direttamente basata sul numero di monitoraggi di cui è stato effettuato il provisioning nella macchina virtuale. La quantità di memoria video riservata varia a seconda della risoluzione dello schermo del sistema e del numero di monitoraggi disponibili. Alcuni utenti richiedono una risoluzione dello schermo superiore per attività specifiche, ma è disponibile una maggiore scalabilità con le impostazioni di risoluzione inferiore se tutte le altre impostazioni rimangono costanti.

### <a name="host-cpu"></a>CPU host

L'hypervisor pianifica l'host e le macchine virtuali sulla CPU. L'overhead viene aumentato in un host abilitato per RemoteFX, perché il sistema esegue un processo aggiuntivo (Rdvgm. exe) per desktop virtuale abilitato per vGPU. Questo processo usa il driver di dispositivo grafico per eseguire i comandi nella GPU. Il codec usa anche la CPU per comprimere i dati dello schermo che devono essere restituiti al client.

Un maggior numero di processori virtuali significa un'esperienza utente migliore. Si consiglia di allocare almeno due CPU virtuali per ogni desktop virtuale abilitato per vGPU. Si consiglia anche di usare l'architettura x64 per i desktop virtuali abilitati per vGPU, perché le prestazioni sulle macchine virtuali x64 sono migliori rispetto alle macchine virtuali x86.

### <a name="gpu-processing-power"></a>Potenza di elaborazione GPU

Ogni desktop virtuale abilitato per vGPU dispone di un processo DirectX corrispondente eseguito sul server host. Questo processo riproduce tutti i comandi di grafica ricevuti dal desktop virtuale RemoteFX sulla GPU fisica. Questa operazione è analoga all'esecuzione di più applicazioni DirectX nello stesso momento nella stessa GPU fisica.

In genere, i dispositivi e i driver grafici sono ottimizzati per eseguire solo alcune applicazioni sul desktop alla volta, ma RemoteFX estende le GPU per proseguire ulteriormente. vGPUs sono dotati di contatori delle prestazioni che misurano la risposta GPU alle richieste RemoteFX e consentono di verificare che le GPU non siano allungate troppo a lungo.

Quando una GPU non dispone di risorse sufficienti, il completamento delle operazioni di lettura e scrittura richiede molto tempo. Gli amministratori possono utilizzare i contatori delle prestazioni per stabilire quando modificare le risorse e impedire tempi di inattività per gli utenti.

Altre informazioni sui contatori delle prestazioni per il monitoraggio del comportamento vGPU di RemoteFX in [diagnosticare problemi di prestazioni grafica in Desktop remoto](https://docs.microsoft.com/azure/virtual-desktop/remotefx-graphics-performance-counters).
