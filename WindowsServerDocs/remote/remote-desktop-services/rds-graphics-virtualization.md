---
title: Servizi Desktop remoto, la tecnologia di virtualizzazione della grafica è adatta a te?
description: Informazioni sulla pianificazione per poter scegliere la grafica corretta opzione di virtualizzazione per la distribuzione di servizi desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/16/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d6ff5b22-7695-4fee-b1bd-6c9dce5bd0e8
author: lizap
manager: scottman
ms.openlocfilehash: 7cf7fdf3510fcaaa955bd0031fb3564fe4372472
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875802"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>La tecnologia di virtualizzazione della grafica è adatta a te?

Hai una gamma di opzioni se si desidera abilitare per il rendering della grafica in Servizi Desktop remoto. Quando si pianifica l'ambiente di virtualizzazione, le considerazioni seguenti unità quali è scegliere la tecnologia di rendering della grafica:

![Considerazioni sul rendering della grafica - Confronta utente scalabilità e i requisiti di GPU per determinare la migliore tecnologia GPU per l'ambiente](media/rds-gpu.png)

In Windows Server 2016, sono disponibili due tecnologie di virtualizzazione grafica disponibile con Hyper-V che consentono di sfruttare l'hardware della GPU:

- [Assegnazione dispositivo discreti (DDA)](#discrete-device-assignment) : per prestazioni più elevate utilizzando uno o più GPU dedicata a una VM che fornisce supporto per GPU nativo all'interno della VM. La densità è bassa poiché è limitata dal numero di GPU fisici disponibili nel server. 
- [VGPU FX remota](#remotefx-vgpu) : per Information worker e scenari di burst elevata GPU in cui più macchine virtuali sfruttano una o più GPU tramite paravirtualizzazione. Questa soluzione offre un aumento della densità utente per ogni server.

La figura seguente mostra gli elementi grafici di opzioni di virtualizzazione in Windows Server 2016.

![Opzioni di virtualizzazione di grafica in Windows Server 2016 con Servizi Desktop remoto - Mostra i tre tecnologie disponibili e le differenze nella scalabilità e prestazioni](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>Assegnazione di dispositivi discreti
Assegnazione dispositivo discreti (DDA) è una soluzione di pass-through hardware che offre le migliori prestazioni, dato che la VM abbia accesso completo alla GPU usando il driver native. L'utente della macchina virtuale può accedere alle funzionalità complete dei loro dispositivi perché anche i driver native del dispositivo. Ciò significa che le caratteristiche e funzionalità di esecuzione del dispositivo in un mirror di macchina virtuale che esegue lo stesso dispositivo sul computer bare metal.

Per ulteriori informazioni su DDA, consultare [pianificare la distribuzione discreti dispositivo assegnazione](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

## <a name="remotefx-vgpu"></a>vGPU RemoteFX 
GPU virtualizzata RemoteFX è una tecnologia di virtualizzazione di grafica che consente la potenza di elaborazione di una GPU per essere suddivise tra diversi sistemi operativi guest per abilitare scenari di lavoro della Knowledge Base (vedere la prima figura precedente). Miglioramenti in Windows Server 2016 consentono ulteriori miglioramenti per gli scenari di burst GPU, ad esempio per la visualizzazione della finestra di progettazione di applicazioni e i dati. Altri miglioramenti apportati alle funzionalità includono:

-   Supporto per la generazione 2 guest, le macchine virtuali, macchine virtuali guest di Windows Server 2016 e Windows Client Hyper-V host.
   >[!NOTE] 
   > Host sessione Desktop remoto non è supportato in Windows Server 2016 guest VM; 1 sola sessione può essere ospitata per ogni macchina virtuale guest Windows Server 2016.

-   Compatibilità delle applicazioni migliorate e la stabilità.
-   VM connettersi modalità sessione avanzata, che consente il reindirizzamento USB e Appunti tramite la connessione della macchina virtuale a una macchina virtuale abilitata per la GPU virtualizzata RemoteFX.

Per altre informazioni, consultare [impostare e configurare GPU virtualizzata RemoteFX per Servizi Desktop remoto](rds-remotefx-vgpu.md).

## <a name="which-should-you-use"></a>Che è opportuno utilizzare?

Considerazioni importanti sulla virtualizzazione di quale tecnologia potrebbe dipendere specifiche hardware o ai requisiti dell'applicazione nell'ambiente in uso. Ecco una breve tabella riguardanti DDA e RemoteFX vGPU funzionalità:

| Funzionalità               | vGPU RemoteFX                                                                       | Assegnazione di dispositivi discreti                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| Assegnazione dispositivo GPU | Paravirtualizzata (numero di macchine virtuali a uno o più GPU)                                     | 1 o più GPU per macchina 1 virtuale                                                  |
| Scala                 | Migliore scalabilità / 1 al numero di macchine virtuali GPU                                                      | Memoria insufficiente. la scala / 1 o più GPU per macchina 1 virtuale                                     |
| Compatibilità delle App     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | Tutte le funzionalità per GPU fornita dal produttore (DX 12, OpenGL, CUDA)          |
| AVC444                | Abilitato per impostazione predefinita (Windows 10 e Windows Server 2016)                             | Disponibile tramite criteri di gruppo (Windows 10 e Windows Server 2016)    |
| GPU VRAM              | Fino a 1 GB dedicati VRAM                                                           | Fino a VRAM supportate dalla GPU                                        |
| Frequenza dei fotogrammi            | Fino a 30fps                                                                         | Fino a 60fps                                                            |
| Driver GPU nel guest   | Driver di visualizzazione della scheda 3D di RemoteFX (Microsoft)                                      | Driver della GPU fornitore (Nvidia, AMD, Intel)                                 |
| Supporto del sistema operativo guest      |  Windows Server 2012 R2  Windows Server 2016  Windows 7 SP1  Windows 8.1 Windows 10 |  Windows Server 2012 R2  Windows Server 2016  Windows 10 Linux         |
| hypervisor            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| Disponibilità del sistema operativo host  |  Windows Server 2012 R2  Windows Server 2016 Windows 10                             | Windows Server 2016                                                    |
| Hardware della GPU          | GPU Enterprise (ad esempio Nvidia Quadro/griglia o AMD FirePro)                         | GPU Enterprise (ad esempio Nvidia Quadro/griglia o AMD FirePro)            |
| Hardware del server       | Nessun requisiti speciali                                                             | Server moderni, espone IOMMU al sistema operativo (in genere SR-IOV conformi hardware) |

Una regola empirica generale consiste nell'usare DDA il migliore compatibilità delle applicazioni, poiché la macchina virtuale avranno accesso diretto alla GPU. Se le applicazioni o carichi di lavoro non è come requisiti rigorosi per GPU e si desidera server una base più ampia di utenti, GPU virtualizzata RemoteFX potrebbe funzionare meglio per l'utente.