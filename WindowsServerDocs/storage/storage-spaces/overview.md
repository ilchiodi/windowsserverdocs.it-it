---
title: Panoramica di spazi di archiviazione
ms.prod: windows-server
ms.author: jgerend
manager: dougkim
ms.technology: storage-file-systems
ms.topic: article
author: jasongerend
ms.date: 05/22/2018
ms.openlocfilehash: 6785508704ff1eebcfd9b70a529ba9d615e5ce11
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858804"
---
# <a name="storage-spaces-overview"></a>Panoramica di spazi di archiviazione

Spazi di archiviazione è una tecnologia di Windows e Windows Server che consente di proteggere i dati dagli errori delle unità. È concettualmente simile a RAID, implementato nel software. È possibile usare spazi di archiviazione per raggruppare tre o più unità in un pool di archiviazione e quindi usare la capacità del pool per creare spazi di archiviazione. In genere archiviano copie aggiuntive dei dati, in modo che in caso di errore di una delle unità, la copia dei dati sia ancora intatta. Se la capacità è insufficiente, è sufficiente aggiungere altre unità al pool di archiviazione.

Esistono quattro modi principali per usare spazi di archiviazione:

- **In un PC Windows** -per altre informazioni, vedere [spazi di archiviazione in Windows 10](https://windows.microsoft.com/windows-10/storage-spaces-windows-10).
- Per altre informazioni, vedere [distribuire spazi di archiviazione in un](deploy-standalone-storage-spaces.md)server autonomo in un server autonomo **con tutte le funzionalità di archiviazione in un server singolo** .
- Per altre informazioni, vedere [spazi di archiviazione diretta Panoramica](storage-spaces-direct-overview.md) **su un server in cluster che usa spazi di archiviazione diretta con archiviazione diretta locale in ogni nodo del cluster** .
- **In un server del cluster con una o più enclosure di archiviazione SAS condivise che contengano tutte le unità** : per altre informazioni, vedere [spazi di archiviazione in un cluster con una panoramica di SAS condivisa](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v%3dws.11)).

