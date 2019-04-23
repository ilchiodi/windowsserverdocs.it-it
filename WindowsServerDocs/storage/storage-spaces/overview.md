---
title: Panoramica di spazi di archiviazione
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: 9977bb35be3676e31cdcab7322b5b5a2cfc67609
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832912"
---
# <a name="storage-spaces-overview"></a>Panoramica di spazi di archiviazione

Spazi di archiviazione è una tecnologia di Windows e Windows Server che consentono di proteggere i dati da errori del disco. È concettualmente simile a RAID, implementato nel software. È possibile usare spazi di archiviazione per raggruppare le unità di tre o più tra loro in un pool di archiviazione e quindi usare la capacità da tale pool per creare spazi di archiviazione. Questi in genere archiviano copie aggiuntive dei dati in modo che se una delle unità non riesce, sarà tuttavia ancora possibile un'invariati copia dei dati. Se capacità insufficiente, è sufficiente aggiungere altre unità al pool di archiviazione.

Esistono quattro aspetti principali per usare spazi di archiviazione:

- **In un PC Windows** : per altre informazioni, vedere [spazi di archiviazione in Windows 10](http://windows.microsoft.com/en-us/windows-10/storage-spaces-windows-10).
- **In un server autonomo con tutti gli archivi in un server singolo** : per altre informazioni, vedere [distribuire spazi di archiviazione in un server autonomo](deploy-standalone-storage-spaces.md).
- **In un server cluster usando spazi di archiviazione diretta con archiviazione locale e collegato direttamente in ogni nodo del cluster** : per altre informazioni, vedere [Panoramica di spazi di archiviazione diretta](storage-spaces-direct-overview.md).
- **In un server in cluster con uno o più enclosure archiviazione SAS condivisi che contiene tutte le unità** : per altre informazioni, vedere [spazi di archiviazione in un cluster con panoramica SAS condivisa](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v%3dws.11)).

