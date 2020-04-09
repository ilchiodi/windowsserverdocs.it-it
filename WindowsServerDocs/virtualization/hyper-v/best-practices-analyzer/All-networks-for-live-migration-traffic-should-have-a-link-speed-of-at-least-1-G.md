---
title: Tutte le reti per il traffico di migrazione in tempo reale devono avere una velocità di collegamento di almeno 1 Gbps
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 89411b63-bec8-463d-b486-107548ed440e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: adb16b1c4618e0874f48f4715440a9d903f5bc9f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857844"
---
# <a name="all-networks-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Tutte le reti per il traffico di migrazione in tempo reale devono avere una velocità di collegamento di almeno 1 Gbps

>Si applica a: Windows Server 2016


  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Nessuna delle reti per il traffico di migrazione in tempo reale ha una velocità di collegamento di almeno 1 Gbps.*  
  
## <a name="impact"></a>Impatto  
*Le migrazioni in tempo reale possono verificarsi lentamente, che potrebbero compromettere la connessione di rete a causa di un timeout di connessione TCP.*  
  
## <a name="resolution"></a>Risoluzione  
*Configurare almeno una rete di migrazione in tempo reale con una velocità di 1 Gbps o superiore.*  
  
Vedere la documentazione del fornitore dell'hardware di rete per verificare se una delle schede di rete esistenti è in grado di supportare una velocità di collegamento di almeno 1 Gbps.  
  


