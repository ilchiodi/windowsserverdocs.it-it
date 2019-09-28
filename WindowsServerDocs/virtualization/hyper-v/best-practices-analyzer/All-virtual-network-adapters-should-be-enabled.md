---
title: Tutte le schede di rete virtuali devono essere abilitate
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fce564fdb47d0677b36078f3d8446579bc06816c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366586"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>Tutte le schede di rete virtuali devono essere abilitate

>Si applica a: Windows Server 2016


  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Una o più schede di rete virtuali associate a una scheda di rete fisica sono disabilitate nel sistema operativo di gestione.*  
  
## <a name="impact"></a>Impatto  
  
*La configurazione del server non è ottimale.*  
  
Il sistema operativo di gestione non è in grado di connettersi a una rete fisica (esterna) utilizzando una delle schede di rete fisiche in questo computer perché è associato a una scheda di rete virtuale disabilitata.  
  
## <a name="resolution"></a>Risoluzione  
  
*Use rete & impostazioni Internet per abilitare la scheda di rete virtuale. In alternativa, usare gestione commutiri virtuali per riconfigurare il Commuter virtuale esterno in modo che non venga condiviso con il sistema operativo di gestione.*  
  


