---
title: Tutte le schede di rete virtuale devono essere abilitate
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0a769c3203f6c6946f01cd91b66fbec38af83bbd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837132"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>Tutte le schede di rete virtuale devono essere abilitate

>Si applica a: Windows Server 2016


  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Una o più schede di rete virtuale associate a una scheda di rete fisiche sono disabilitate nel sistema operativo di gestione.*  
  
## <a name="impact"></a>Impatto  
  
*La configurazione di questo server non è ottimale.*  
  
Sistema operativo di gestione non è possibile connettersi a una rete fisica (esterna) utilizzando una delle schede di rete fisiche nel computer perché l'associazione a una scheda di rete virtuali disattivate.  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare rete e Internet per abilitare la scheda di rete virtuale. In alternativa, utilizzare Gestione commutatori virtuali per riconfigurare il commutatore virtuale esterno in modo che non viene condivisa con il sistema operativo di gestione.*  
  


