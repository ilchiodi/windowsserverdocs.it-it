---
title: Eseguire il prehashing e il precaricamento del contenuto nel server della cache ospitata (facoltativo)
description: In questa guida vengono fornite istruzioni sulla distribuzione di BranchCache in modalità cache ospitata sul computer che eseguono Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b60a1f24b8988d6e394df0faf678467021e0c882
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839392"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Prehash e a precaricare contenuto nel Server Cache ospitata \(facoltativo\)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile utilizzare le procedure descritte in questa sezione per prehash contenuto sui server di contenuti, aggiungere il contenuto per i pacchetti di dati e quindi precaricare il contenuto nei server cache ospitata. 

Queste procedure sono facoltative in quanto non implica la necessità di precaricamento e prehash contenuto nei server cache ospitata. 

Se è non precaricare contenuto, i dati viene aggiunto alla cache ospitata automaticamente come client scaricano tramite una connessione WAN.

>[!IMPORTANT]
>Anche se queste procedure sono collettivamente facoltative, se si decide di precaricamento e prehash contenuto sui server cache ospitata, è necessario eseguire entrambe le procedure.

- [Creare pacchetti di dati contenuto Server per il Web e contenuto del File &#40;facoltativo&#41;](8-Bc-Data-Packages.md)
  
- [Importare i pacchetti di dati nel Server Cache ospitata &#40;facoltativo&#41;](9-Bc-Import-Data.md)

Per continuare con questa Guida, vedere [creare dei pacchetti di dati contenuto Server per Web e File di contenuto & #40; facoltativo & #41;](8-Bc-Data-Packages.md).