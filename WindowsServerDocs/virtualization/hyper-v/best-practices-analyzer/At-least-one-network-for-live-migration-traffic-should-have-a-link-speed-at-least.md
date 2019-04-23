---
title: Almeno una rete per il traffico di migrazione in tempo reale deve avere una velocità di collegamento di almeno 1 Gbps
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5714df3f-f810-4618-8c93-e24881651100
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ef75f73bd934b863b146e93f4cdc7323e5d4c6fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837732"
---
# <a name="at-least-one-network-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Almeno una rete per il traffico di migrazione in tempo reale deve avere una velocità di collegamento di almeno 1 Gbps

>Si applica a: Windows Server 2016


  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Nessun delle reti per il traffico di migrazione in tempo reale con una velocità di collegamento pari ad almeno 1 Gbps.*  
  
## <a name="impact"></a>Impatto  
*Migrazioni in tempo reale potrebbero verificarsi lenta, che può interrompere la connessione di rete a causa di un timeout di connessione TCP.*  
  
## <a name="resolution"></a>Risoluzione  
*Configurare almeno una rete di migrazione in tempo reale con una velocità pari a 1 Gbps o più veloce.*  
  
Vedere la documentazione del fornitore di hardware di rete per scoprire se una delle schede di rete esistente può supportare una velocità di collegamento pari ad almeno 1 Gbps.  
  


