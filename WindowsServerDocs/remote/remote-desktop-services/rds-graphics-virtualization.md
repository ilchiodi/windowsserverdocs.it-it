---
title: Servizi Desktop remoto - Accelerazione GPU
description: Informazioni sulla pianificazione per scegliere l'opzione di virtualizzazione della grafica corretta per la propria distribuzione di Servizi Desktop remoto (RDS).
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/21/2019
ms.topic: article
ms.assetid: d6ff5b22-7695-4fee-b1bd-6c9dce5bd0e8
author: lizap
manager: scottman
ms.openlocfilehash: 5403be897df48972edd4c5b37744a83d8ad25972
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861384"
---
# <a name="remote-desktop-services---gpu-acceleration"></a>Servizi Desktop remoto - Accelerazione GPU

Servizi Desktop remoto funziona sia con l'accelerazione grafica nativa sia con le tecnologie di virtualizzazione grafica supportate da Windows Server. Per informazioni su queste tecnologie, sulle relative differenze e su come distribuirle, vedi [Pianificare l'accelerazione GPU in Windows Server](../../virtualization/hyper-v/plan/plan-for-gpu-acceleration-in-windows-server.md).

Quando pianifichi l'accelerazione grafica nell'ambiente Servizi Desktop remoto, la scelta della tecnologia di rendering per la grafica dipenderà della scalabilità degli utenti e dai carichi di lavoro degli utenti:

![Considerazioni sul rendering della grafica: confronto tra la scalabilità dell'utente e i requisiti di GPU per determinare la migliore tecnologia GPU per l'ambiente](media/rds-gpu.png)
