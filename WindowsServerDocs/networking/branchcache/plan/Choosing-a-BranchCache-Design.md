---
title: Scelta di una progettazione di BranchCache
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 330dcbee26f52ff69cd85ef8dc78d2e161b943d1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811912"
---
# <a name="choosing-a-branchcache-design"></a>Scelta di una progettazione di BranchCache

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Per informazioni sulle modalità di BranchCache e per selezionare le modalità migliori per la distribuzione, è possibile utilizzare questo argomento.  
  
È possibile utilizzare questa Guida alla distribuzione di BranchCache nelle modalità seguenti e combinazioni di modalità.  
  
-   Tutte le filiali sono configurate per la modalità cache distribuita.  
  
-   Tutte le filiali sono configurate per la modalità cache ospitata e dispone di un server cache ospitata nel sito.  
  
-   Alcune succursali sono configurati per la modalità cache distribuita e alcune succursali dispone di un server cache ospitata nel sito e sono configurati per la modalità cache ospitata.  
  
Nella figura seguente viene illustrata un'installazione in modalità doppia, con un ramo configurato per la modalità cache distribuita e una succursale configurato per la modalità cache ospitata.  
  
![Scelta di una progettazione di BranchCache](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)  
  
Prima di distribuire BranchCache, selezionare la modalità desiderata per ogni filiale dell'organizzazione.  
  


