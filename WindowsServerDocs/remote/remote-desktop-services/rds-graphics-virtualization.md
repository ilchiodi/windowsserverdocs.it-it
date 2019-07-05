---
title: Servizi Desktop remoto - Qual è la tecnologia per la virtualizzazione della grafica più adatta?
description: Informazioni sulla pianificazione per scegliere l'opzione di virtualizzazione della grafica corretta per la propria distribuzione di Servizi Desktop remoto (RDS).
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
ms.openlocfilehash: af5d5ce89561c89d8468627e20dfdb6f35eca5ef
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66447115"
---
# <a name="which-graphics-virtualization-technology-is-right-for-you"></a>Qual è la tecnologia per la virtualizzazione della grafica più adatta?

Esistono diverse opzioni se si vuole abilitare il rendering della grafica in Servizi Desktop remoto. Quando si pianifica l'ambiente di virtualizzazione, la scelta della tecnologia di rendering della grafica è definita dalle considerazioni seguenti:

![Considerazioni sul rendering della grafica: confronto tra la scalabilità dell'utente e i requisiti di GPU per determinare la migliore tecnologia GPU per l'ambiente](media/rds-gpu.png)

In Windows Server 2016, sono disponibili due tecnologie di virtualizzazione della grafica con Hyper-V che consentono di sfruttare l'hardware della GPU:

- [Assegnazione di dispositivi discreti (DDA)](#discrete-device-assignment): per avere prestazioni molto elevate usando una o più GPU dedicate a una VM per offrire supporto del driver nativo della GPU all'interno della VM. La densità è bassa perché è limitata dal numero di GPU fisiche disponibili nel server. 
- [FX vGPU remota](#remotefx-vgpu): per knowledge worker e scenari della GPU con burst elevato in cui più VM sfruttano una o più GPU tramite para-virtualizzazione. Questa soluzione assicura un aumento della densità utente per ogni server.

La figura seguente mostra le opzioni di virtualizzazione della grafica in Windows Server 2016.

![Opzioni di virtualizzazione della grafica in Windows Server 2016 con Servizi Desktop remoto: mostra le tre tecnologie disponibili e le differenze in scalabilità e prestazioni](media/rds-graphics-virtualization.png)

## <a name="discrete-device-assignment"></a>Assegnazione di dispositivi discreti
L'assegnazione di dispositivi discreti (DDA) è una soluzione di pass-through hardware che offre le migliori prestazioni, in quanto la VM ha accesso completo alla GPU usando il driver nativo. L'utente della VM può accedere alle funzionalità complete dei suoi dispositivi come il driver nativo del dispositivo. Questo significa che le caratteristiche e funzionalità del dispositivo eseguito in una VM rispecchiano l'esecuzione dello stesso dispositivo bare metal.

Per altre informazioni sulla DDA, consultare [Pianificare la distribuzione dell'assegnazione di dispositivi discreti](../../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).

## <a name="remotefx-vgpu"></a>RemoteFX vGPU 
RemoteFX vGPU è una tecnologia di virtualizzazione della grafica che garantisce di suddividere la potenza di elaborazione di una GPU in diversi sistemi operativi guest per abilitare scenari di knowledge worker (vedere la prima figura in alto). I progressi di Windows Server 2016 apportano ulteriori miglioramenti per gli scenari di burst della GPU, ad esempio per le applicazioni della finestra di progettazione e la visualizzazione dei dati. Altri principali miglioramenti includono:

- Supporto per VM guest di seconda generazione, VM guest di Windows Server 2016 e host Windows Client Hyper-V.
  >[!NOTE] 
  > L'host di sessione Desktop remoto non è supportato sulle VM host di Windows Server 2016. Può essere ospitata solo una sessione per VM guest di Windows Server 2016.

- Compatibilità e stabilità delle applicazioni migliorate.
- VM Connect Enhanced Session Mode, che consente il reindirizzamento USB e degli appunti tramite la connessione di una VM a un'altra VM abilitata per RemoteFX vGPU.

Per altre informazioni, consultare [Installare e configurare RemoteFX vGPU per Servizi Desktop remoto](rds-remotefx-vgpu.md).

## <a name="which-should-you-use"></a>Quale usare?

Le considerazioni importanti sulla scelta della tecnologia di virtualizzazione potrebbero dipendere dalle specifiche hardware o dai requisiti dell'applicazione nell'ambiente in uso. Ecco una breve tabella sulle funzionalità di DDA e RemoteFX vGPU:

| Funzionalità               | RemoteFX vGPU                                                                       | Assegnazione di dispositivi discreti                                             |
|-----------------------|-------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| Assegnazione GPU del dispositivo | Paravirtualizzata (molte VM a una o più GPU)                                     | 1 o più GPU a 1 VM                                                  |
| Scala                 | Migliore scalabilità / 1 GPU a molte VM                                                      | Scalabilità bassa / 1 o più GPU a 1 VM                                     |
| Compatibilità delle app     | DX 11.1, OpenGL 4.4, OpenCL 1.1                                                     | Tutte le funzionalità della GPU offerte dal fornitore (DX 12, OpenGL, CUDA)          |
| AVC444                | Abilitato per impostazione predefinita (Windows 10 e Windows Server 2016)                             | Disponibile tramite Criteri di gruppo (Windows 10 e Windows Server 2016)    |
| VRAM della GPU              | Fino a 1 GB di VRAM dedicata                                                           | Fino a livello di VRAM supportata dalla GPU                                        |
| Frequenza dei fotogrammi            | Fino a 30 fps                                                                         | Fino a 60 fps                                                            |
| Driver GPU nel guest   | Driver display adattatore 3D RemoteFX (Microsoft)                                      | Driver della GPU del fornitore (Nvidia, AMD, Intel)                                 |
| Supporto del sistema operativo guest      |  Windows Server 2012 R2, Windows Server 2016, Windows 7 SP1, Windows 8.1, Windows 10 |  Windows Server 2012 R2, Windows Server 2016, Windows 10, Linux         |
| Hypervisor            | Microsoft Hyper-V                                                                   | Microsoft Hyper-V                                                      |
| Disponibilità del sistema operativo host  |  Windows Server 2012 R2, Windows Server 2016, Windows 10                             | Windows Server 2016                                                    |
| Hardware della GPU          | GPU enterprise (ad esempio Nvidia Quadro/GRUD o AMD FirePro)                         | GPU enterprise (ad esempio Nvidia Quadro/GRUD o AMD FirePro)            |
| Hardware del server       | Nessun requisito speciale                                                             | Server moderno, espone IOMMU al sistema operativo (in genere hardware compatibile con SR-IOV) |

Una regola generica consiste nell'usare la DDA per avere la migliore compatibilità delle applicazioni, perché la VM avrà accesso diretto alla GPU. Se le applicazioni o i carichi di lavoro non hanno requisiti di GPU severi e si vuole servire una base più ampia di utenti, RemoteFX vGPU potrebbe la soluzione migliore.