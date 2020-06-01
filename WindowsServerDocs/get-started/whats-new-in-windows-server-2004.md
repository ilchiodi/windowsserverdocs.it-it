---
title: Novità di Windows Server, versione 2004
description: Nuove funzionalità in Windows Server, versione 2004
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: Heidilohr
ms.author: helohr
ms.date: 05/27/2020
ms.localizationpriority: high
ms.openlocfilehash: e0136dad7180e41f15ae6226008aa7580ec53283
ms.sourcegitcommit: c63672805c93d5bf2a9eb71b3e2de2df00194529
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2020
ms.locfileid: "84124899"
---
# <a name="whats-new-in-windows-server-version-2004"></a>Novità di Windows Server, versione 2004

>Si applica a: Windows Server (Canale semestrale)

Per informazioni sulle funzionalità più recenti di Windows, vedi [Novità di Windows Server](whats-new-in-windows-server.md). Questo argomento descrive alcune delle nuove funzionalità in Windows Server, versione 2004.

## <a name="server-core-container-improvements"></a>Miglioramenti al contenitore Server Core

Sono state ridotte le dimensioni complessive delle immagini del contenitore di Server Core per migliorare la velocità e le prestazioni di download. Sono stati inclusi i miglioramenti seguenti:

- Sono state rimosse dall'immagine del contenitore di Server Core la maggior parte delle immagini NGEN per ridurre le dimensioni dell'immagine.
- Le immagini di runtime .NET Framework basate sulle immagini del contenitore Server Core sono ora ottimizzate per le app ASP.NET e le prestazioni degli script di Windows PowerShell.
- Il team .NET ha anche verificato che sia presente una sola copia di ogni immagine NGEN, riducendo le dimensioni delle immagini di .NET Framework.

Per dare un'idea più approfondita delle dimensioni di questi contenitori, nella tabella seguente viene confrontata la versione corrente del contenitore relativo all'[aggiornamento della sicurezza mensile di maggio 2020](https://support.microsoft.com/help/4561769/windows-server-containers-for-may-2020) (noto anche come aggiornamento "5B") con le versioni precedenti.

| Versione del contenitore | Dimensione del download | Dimensioni su disco |
|---|---|---|
| Windows Server, versione 1903 | 2,311 GB | 5,1 GB |
| Windows Server, versione 1909 | 2,257 GB | 4,97 GB |
| Windows Server, versione 2004 | 1,830 GB | 3,98 GB |

Per ulteriori informazioni sull'aggiornamento di Windows Server, versione 2004, vedere [il post di blog](https://techcommunity.microsoft.com/t5/containers/windows-server-version-2004-now-available/ba-p/1419194). Per ulteriori informazioni sugli aggiornamenti dei contenitori di Windows in generale, vedere [Aggiornare i contenitori di Windows Server](/virtualization/windowscontainers/deploy-containers/update-containers/).
