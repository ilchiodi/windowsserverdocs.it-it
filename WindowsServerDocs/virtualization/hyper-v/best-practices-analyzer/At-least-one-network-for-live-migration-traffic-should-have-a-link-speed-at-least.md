---
title: Almeno una rete per il traffico della migrazione in tempo reale deve avere una velocità di collegamento di almeno 1 Gbps
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5714df3f-f810-4618-8c93-e24881651100
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 04ce4ea86e39e8bd98216ae4e6b12899c9366421
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857824"
---
# <a name="at-least-one-network-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Almeno una rete per il traffico della migrazione in tempo reale deve avere una velocità di collegamento di almeno 1 Gbps

>Si applica a: Windows Server 2016


  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Nessuna delle reti per il traffico di migrazione in tempo reale ha una velocità di collegamento di almeno 1 Gbps.*  
  
## <a name="impact"></a>Impatto  
*Le migrazioni in tempo reale possono verificarsi lentamente, che potrebbero compromettere la connessione di rete a causa di un timeout di connessione TCP.*  
  
## <a name="resolution"></a>Risoluzione  
*Configurare almeno una rete di migrazione in tempo reale con una velocità di 1 Gbps o superiore.*  
  
Vedere la documentazione del fornitore dell'hardware di rete per verificare se una delle schede di rete esistenti è in grado di supportare una velocità di collegamento di almeno 1 Gbps.  
  


