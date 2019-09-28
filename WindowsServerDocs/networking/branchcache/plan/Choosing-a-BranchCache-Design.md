---
title: Scelta di una progettazione di BranchCache
description: Questo argomento fa parte di BranchCache distribuzione Guide per Windows Server 2016, che illustra come distribuire BranchCache in modalità cache distribuita e ospitato per ottimizzare l'utilizzo della larghezza di banda WAN nelle succursali
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a1b4615dfda3989f0321725fd27da066fcb8a5e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406312"
---
# <a name="choosing-a-branchcache-design"></a>Scelta di una progettazione di BranchCache

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Per informazioni sulle modalità di BranchCache e per selezionare le modalità migliori per la distribuzione, è possibile utilizzare questo argomento.  
  
È possibile utilizzare questa Guida alla distribuzione di BranchCache nelle modalità seguenti e combinazioni di modalità.  
  
-   Tutte le filiali sono configurate per la modalità cache distribuita.  
  
-   Tutte le filiali sono configurate per la modalità cache ospitata e dispone di un server cache ospitata nel sito.  
  
-   Alcune succursali sono configurati per la modalità cache distribuita e alcune succursali dispone di un server cache ospitata nel sito e sono configurati per la modalità cache ospitata.  
  
Nella figura seguente viene illustrata un'installazione in modalità doppia, con un ramo configurato per la modalità cache distribuita e una succursale configurato per la modalità cache ospitata.  
  
![Scelta di una progettazione di BranchCache](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)  
  
Prima di distribuire BranchCache, selezionare la modalità desiderata per ogni filiale dell'organizzazione.  
  


