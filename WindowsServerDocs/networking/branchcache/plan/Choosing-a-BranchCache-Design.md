---
title: Scelta di una progettazione di BranchCache
description: In questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4fe40b3d9ece771a46af8ecc70297b8713d65875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="choosing-a-branchcache-design"></a>Scelta di una progettazione di BranchCache

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sulle modalità di BranchCache e per selezionare le modalità migliori per la distribuzione.  
  
È possibile utilizzare questa Guida alla distribuzione di BranchCache nelle modalità seguenti e combinazioni di modalità.  
  
-   Tutte le filiali sono configurate per la modalità cache distribuita.  
  
-   Tutte le filiali sono configurate per la modalità cache ospitata e dispone di un server cache ospitata nel sito.  
  
-   Alcune succursali sono configurati per la modalità cache distribuita e alcune succursali dispone di un server cache ospitata nel sito e sono configurati per la modalità cache ospitata.  
  
La figura seguente illustra un'installazione in modalità doppia, con una succursale configurato per la modalità cache distribuita e una succursale configurato per la modalità cache ospitata.  
  
![Scelta di una progettazione di BranchCache](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)  
  
Prima di distribuire BranchCache, selezionare la modalità desiderata per ogni filiale dell'organizzazione.  
  


