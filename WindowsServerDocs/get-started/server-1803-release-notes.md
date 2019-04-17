---
title: 'Note sulla versione: problemi importanti di Windows Server, versione 1803'
description: Informazioni sui problemi noti, limitazioni o altre informazioni che necessarie prima di installare Windows Server, versione 1803
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 05/07/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: e9bd860769ec375a6d89ac452e3430b791fff3ad
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339319"
---
# Note sulla versione: Problemi importanti di Windows Server, versione 1803

>Si applica a: Canale semestrale di Windows Server

Queste note sulla versione presentano una sintesi dei problemi più critici nel sistema operativo Windows Server, incluse le soluzioni per evitare o risolvere i problemi noti. Per informazioni sulle nuove funzionalità in questa versione, vedi [What ' s New in Windows Server versione 1803](whats-new-in-windows-server-1803.md). Controlla i [contenitori di Windows su](https://docs.microsoft.com/virtualization/windowscontainers/about/) se sei interessato a che esegue Windows Server, versione 1803, contenitore. 

Se non diversamente specificato, ogni problema segnalato è applicabile a tutte le edizioni e opzioni di installazione di Windows Server, versione 1803.  

Questo articolo ti continuamente aggiornato. Se vengono identificati i problemi noti, ti verrà documentarli qui. 


## Data Center software-defined

Funzionalità di Data Center software-defined come spazi di archiviazione diretta, definita da software di rete e le schermate macchine virtuali non sono incluse in Windows Server, versione 1803. Come descritto nel [canale semestrale di Windows Server update](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/), il canale semestrale di Windows Server è incentrato sui contenitori e scenari di applicazioni beneficiano innovazione più rapida. 

Se hai bisogno di ruoli dell'infrastruttura, utilizzare una versione Long-Term Servicing Channel: Windows Server 2016 (ora disponibile) o [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (in arrivo nel corso dell'anno).

Il nostro impegno per la creazione della piattaforma ottimale per l'infrastruttura iperconvergente e continuare a sviluppare nuove funzionalità e migliorare quelli esistenti in base al tuo feedback. Grazie per che ci aiuta a ottenere a [10.000 + cluster di spazi di archiviazione diretta](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum). Vedremo in avanti per la condivisione più entro l'anno.