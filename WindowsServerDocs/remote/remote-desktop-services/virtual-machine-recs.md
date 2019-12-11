---
title: Dimensionamento delle macchine virtuali
description: Indicazioni relative alle dimensioni per ogni tipo di carico di lavoro.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/02/2019
ms.tgt_pltfrm: na
ms.topic: article
author: Heidilohr
manager: daveba
ms.openlocfilehash: 964dba2fc1a3cc1cf0e9cfe2392d40b9ea8f5ece
ms.sourcegitcommit: cbf0c7c37797c22af989639fac82fc0eee94497f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2019
ms.locfileid: "74700910"
---
# <a name="virtual-machine-sizing-guidance"></a>Linee guida relative al dimensionamento delle macchine virtuali

Indipendentemente dal fatto che la tua macchina virtuale sia in esecuzione in Servizi Desktop remoto o Desktop virtuale di Windows, tipi di carichi di lavoro diversi richiedono diverse configurazioni di macchine virtuali (VM) host di sessione. Per ottenere la migliore esperienza possibile, ridimensiona la distribuzione in base alle esigenze dei tuoi utenti.

## <a name="multi-session-recommendations"></a>Indicazioni per scenari multisessione

La tabella seguente indica il numero massimo di utenti suggerito per ogni CPU virtuale (vCPU) e la configurazione minima delle macchine virtuali per ogni carico di lavoro. Queste indicazioni sono basate sui [carichi di lavoro di Desktop remoto](remote-desktop-workloads.md).

| Tipo di carico di lavoro | Numero massimo di utenti per vCPU | Spazio di archiviazione minimo vCPU/RAM/sistema operativo | Istanze di Azure di esempio | Spazio di archiviazione minimo contenitore profili |
| --- | --- | --- | --- | --- |
| Chiaro | 6 | 2 vCPU, 8 GB di RAM, 16 GB di spazio di archiviazione | D2s_v3, F2s_v2 | 30 GB |
| Medio | 4 | 4 vCPU, 16 GB di RAM, 32 GB di spazio di archiviazione | D4s_v3, F4s_v2 | 30 GB |
| Pesante | 2 | 4 vCPU, 16 GB di RAM, 32 GB di spazio di archiviazione | D4s_v3, F4s_v2 | 30 GB |
| Alimentazione | 1 | 6 vCPU, 56 GB di RAM, 340 GB di spazio di archiviazione | D4s_v3, F4s_v2, NV6 | 30 GB |

## <a name="single-session-recommendations"></a>Indicazioni per scenari a sessione singola

Per indicazioni sul dimensionamento delle macchine virtuali per scenari a sessione singola, è consigliabile avere almeno due core CPU fisici per macchina virtuale (in genere, quattro vCPU con hyperthreading). Se sono necessarie indicazioni più specifiche per il dimensionamento delle macchine virtuali per scenari a sessione singola, rivolgiti ai fornitori di software specifici per il tuo carico di lavoro. Il dimensionamento delle macchine virtuali a sessione singola è probabilmente in linea con le linee guida relative ai dispositivi fisici.

## <a name="general-virtual-machine-recommendations"></a>Indicazioni generali relative alle macchine virtuali

Per i requisiti delle macchine virtuali per l'esecuzione del sistema operativo, vedi [Specifiche e requisiti di sistema dei computer Windows 10](https://www.microsoft.com/windows/windows-10-specifications).

Per i carichi di lavoro di produzione che richiedono un contratto di servizio (SLA) è consigliabile usare la risorsa di archiviazione Premium SSD nel disco del sistema operativo. Per altri dettagli, vedi il [contratto di servizio per le macchine virtuali](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/).

Le unità di elaborazione grafica (GPU) rappresentano una buona scelta per gli utenti che usano regolarmente programmi con uso elevato di grafica per rendering video, progettazione 3D e simulazioni. Per altre informazioni sull'accelerazione grafica, vedi [Scegliere la tecnologia per il rendering della grafica](rds-graphics-virtualization.md). Azure offre diverse opzioni di distribuzione dell'accelerazione grafica e più dimensioni di macchine virtuali disponibili per le GPU. Per altre informazioni, vedi [Dimensioni delle macchine virtuali ottimizzate per le GPU](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

Le [macchine virtuali della serie B con possibilità di burst](https://docs.microsoft.com/azure/virtual-machines/windows/b-series-burstable) sono una buona scelta per gli utenti che non hanno sempre la necessità di prestazioni massime della CPU. Per altre informazioni sui tipi e sulle dimensioni delle macchine virtuali, vedi [Dimensioni per le macchine virtuali Windows in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) e consulta le informazioni sui prezzi alla [pagina Serie di macchine virtuali](https://azure.microsoft.com/pricing/details/virtual-machines/series/).

## <a name="test-your-workload"></a>Testare il carico di lavoro

Infine, ti consigliamo di usare gli strumenti di simulazione per testare la distribuzione sia con test di stress sia con simulazioni di utilizzo reale. Verifica che il sistema sia sufficientemente reattivo e resiliente da soddisfare le esigenze degli utenti e ricorda di variare le dimensioni del carico per evitare sorprese.
