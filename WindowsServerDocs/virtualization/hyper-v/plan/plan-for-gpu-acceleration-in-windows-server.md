---
title: Pianificare l'accelerazione della GPU in Windows Server
description: Informazioni sulle diverse tecnologie Hyper-V per l'accelerazione GPU, tra cui DDA e RemoteFX vGPU
ms.prod: windows-server
ms.reviewer: rickman
author: rick-man
ms.author: rickman
manager: stevelee
ms.topic: article
ms.date: 08/21/2019
ms.openlocfilehash: 7ca8d29b58dc8682575d9cb8b0f26aa49b257335
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80307859"
---
# <a name="plan-for-gpu-acceleration-in-windows-server"></a>Pianificare l'accelerazione della GPU in Windows Server

> Si applica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019 Microsoft Hyper-V Server 2019

Questo articolo presenta le funzionalità di virtualizzazione grafica disponibili in Windows Server.

## <a name="when-to-use-gpu-acceleration"></a>Quando usare l'accelerazione GPU

A seconda del carico di lavoro, può essere opportuno prendere in considerazione l'accelerazione della GPU. Ecco gli elementi da considerare prima di scegliere accelerazione GPU:

- **Carichi di lavoro app e desktop Remoting (VDI/DaaS)** : se si sta creando un'app o un servizio di comunicazione remota desktop con Windows Server, si consideri il catalogo delle app che gli utenti devono eseguire. Alcuni tipi di app, ad esempio le app CAD/CAM, le app per la simulazione, i giochi e le app per il rendering e la visualizzazione, si basano principalmente sul rendering 3D per offrire interattività uniformi e reattive. La maggior parte dei clienti considera le GPU una necessità per un'esperienza utente ragionevole con questi tipi di app.
- **Carichi di lavoro di rendering, codifica e visualizzazione remoti**: questi carichi di lavoro orientati alla grafica tendono a dipendere molto dalle funzionalità specializzate di una GPU, ad esempio il rendering 3D efficiente e la codifica/decodifica dei frame, allo scopo di ottenere obiettivi di convenienza e velocità effettiva. Per questo tipo di carico di lavoro, è possibile che una singola macchina virtuale abilitata per la GPU sia in grado di soddisfare la velocità effettiva di molte VM solo CPU.
- **Carichi di lavoro HPC e ml**: per carichi di lavoro di calcolo altamente dati paralleli, ad esempio la formazione o l'inferenza di modelli di calcolo a prestazioni elevate e di Machine Learning, le GPU possono ridurre notevolmente il tempo per ottenere risultati, tempi di inferenza e tempi di training. In alternativa, possono offrire una migliore convenienza rispetto a un'architettura solo CPU a un livello di prestazioni analogo. Molti Framework di HPC e Machine Learning hanno un'opzione per abilitare l'accelerazione della GPU. valutare se questo potrebbe trarre vantaggio dal carico di lavoro specifico.

## <a name="gpu-virtualization-in-windows-server"></a>Virtualizzazione GPU in Windows Server

Le tecnologie di virtualizzazione GPU consentono l'accelerazione della GPU in un ambiente virtualizzato, in genere all'interno delle macchine virtuali. Se il carico di lavoro è virtualizzato con Hyper-V, sarà necessario usare la virtualizzazione grafica per fornire l'accelerazione GPU dalla GPU fisica alle app o ai servizi virtualizzati. Tuttavia, se il carico di lavoro viene eseguito direttamente negli host Windows Server fisici, non è necessario eseguire la virtualizzazione grafica. le app e i servizi hanno già accesso alle API e alle funzionalità GPU supportate in modo nativo in Windows Server.

Le seguenti tecnologie di virtualizzazione grafica sono disponibili per le macchine virtuali Hyper-V in Windows Server:

- [Assegnazione di dispositivi discreti (DDA)](#discrete-device-assignment-dda)
- [RemoteFX vGPU](#remotefx-vgpu)

Oltre ai carichi di lavoro delle macchine virtuali, Windows Server supporta anche l'accelerazione GPU dei carichi di lavoro in contenitori nei contenitori di Windows. Per ulteriori informazioni, vedere [accelerazione GPU nei contenitori di Windows](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/gpu-acceleration).

## <a name="discrete-device-assignment-dda"></a>Assegnazione di dispositivi discreti (DDA)

L'assegnazione di dispositivi discreti (DDA), nota anche come pass-through GPU, consente di dedicare una o più GPU fisiche a una macchina virtuale. In una distribuzione di DDA, i carichi di lavoro virtualizzati vengono eseguiti sul driver nativo e in genere hanno accesso completo alle funzionalità della GPU. DDA offre il massimo livello di compatibilità delle app e delle prestazioni potenziali. DDA può anche fornire l'accelerazione GPU alle VM Linux, in base al supporto.

Una distribuzione di DDA può accelerare solo un numero limitato di macchine virtuali, perché ogni GPU fisica può fornire l'accelerazione al massimo di una macchina virtuale. Se si sta sviluppando un servizio la cui architettura supporta le macchine virtuali condivise, è consigliabile ospitare più carichi di lavoro accelerati per macchina virtuale. Se, ad esempio, si sta creando un servizio di comunicazione remota desktop con Servizi Desktop remoto, è possibile migliorare la scalabilità utente sfruttando le funzionalità di multi-sessione di Windows Server per ospitare più desktop utente in ogni macchina virtuale. Questi utenti condivideranno i vantaggi dell'accelerazione GPU.

Per altre informazioni, vedi questi argomenti:

- [Pianificare la distribuzione dell'assegnazione di dispositivi discreti](plan-for-deploying-devices-using-discrete-device-assignment.md)
- [Distribuire dispositivi grafici con l'assegnazione di dispositivi discreti](../deploy/Deploying-graphics-devices-using-dda.md)

## <a name="remotefx-vgpu"></a>vGPU RemoteFX

> [!NOTE]
> RemoteFX vGPU è completamente supportato in Windows Server 2016 ma non è supportato in Windows Server 2019.

RemoteFX vGPU è una tecnologia di virtualizzazione grafica che consente la condivisione di una singola GPU fisica tra più macchine virtuali. In una distribuzione di RemoteFX vGPU, i carichi di lavoro virtualizzati vengono eseguiti sulla scheda 3D RemoteFX di Microsoft, che coordina le richieste di elaborazione della GPU tra l'host e i guest. RemoteFX vGPU è particolarmente adatto per carichi di lavoro knowledge worker e con picchi elevati in cui non sono necessarie risorse GPU dedicate. RemoteFX vGPU può fornire solo l'accelerazione GPU alle macchine virtuali Windows.

Per altre informazioni, vedi questi argomenti:

- [Distribuire dispositivi grafici con RemoteFX vGPU](../deploy/deploy-graphics-devices-using-remotefx-vgpu.md)
- [Supporto per la scheda video 3D RemoteFX (vGPU)](../../../remote/remote-desktop-services/rds-supported-config.md#remotefx-3d-video-adapter-vgpu-support)

## <a name="comparing-dda-and-remotefx-vgpu"></a>Confronto tra DDA e RemoteFX vGPU

Quando si pianifica la distribuzione di, considerare le funzionalità seguenti e supportare le differenze tra le tecnologie di virtualizzazione grafica:

|                       | vGPU RemoteFX                                                                       | Assegnazione di dispositivi discreti                                                          |
|-----------------------|-------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| Modello di risorsa GPU    | Dedicato o condiviso                                                                 | Solo dedicato                                                                      |
| Densità VM            | Alta (una o più GPU per molte VM)                                                 | Bassa (una o più GPU in una macchina virtuale)                                                    |
| Compatibilità delle app     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | Tutte le funzionalità della GPU offerte dal fornitore (DX 12, OpenGL, CUDA)                       |
| AVC444                | Abilitato per impostazione predefinita                                                                  | Disponibile tramite Criteri di gruppo                                                      |
| VRAM della GPU              | Fino a 1 GB di VRAM dedicata                                                           | Fino a livello di VRAM supportata dalla GPU                                                     |
| Frequenza dei fotogrammi            | Fino a 30 fps                                                                         | Fino a 60 fps                                                                         |
| Driver GPU nel guest   | Driver display adattatore 3D RemoteFX (Microsoft)                                      | Driver del fornitore GPU (NVIDIA, AMD, Intel)                                              |
| Supporto del sistema operativo host       | Windows Server 2016                                                                 | Windows Server 2016; Windows Server 2019                                            |
| Supporto del sistema operativo guest      | Windows Server 2012 R2; Windows Server 2016; Windows 7 SP1; Windows 8.1; Windows 10 | Windows Server 2012 R2; Windows Server 2016; Windows Server 2019; Windows 10; Linux |
| Hypervisor            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                                   |
| Hardware della GPU          | GPU enterprise (ad esempio Nvidia Quadro/GRUD o AMD FirePro)                         | GPU enterprise (ad esempio Nvidia Quadro/GRUD o AMD FirePro)                         |
| Hardware del server       | Nessun requisito speciale                                                             | Server moderno, espone IOMMU al sistema operativo (in genere hardware compatibile con SR-IOV)              |
