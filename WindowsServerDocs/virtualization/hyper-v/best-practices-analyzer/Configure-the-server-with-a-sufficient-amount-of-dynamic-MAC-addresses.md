---
title: Configurare il server con una quantità sufficiente di indirizzi MAC dinamici
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fc444225c38ef7e8605ec328cfe3f8184b2fd307
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870732"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>Configurare il server con una quantità sufficiente di indirizzi MAC dinamici

>Si applica a: Windows Server 2016

*Questo argomento è dedicato alla risoluzione di un problema specifico identificato da un'analisi di Best Practices Analyzer. È consigliabile applicare le informazioni contenute in questo argomento solo ai computer in cui è stato eseguito Best di Best Practices Analyzer di Hyper-V e si verifica il problema discusso in questo argomento. Per altre informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Il numero di indirizzi MAC dinamici disponibili è basso.*  
  
## <a name="impact"></a>Impatto  
  
*Quando nessun indirizzo MAC dinamico è disponibile, le macchine virtuali configurate per usare un indirizzo MAC dinamico non può essere avviate.*  
  
## <a name="resolution"></a>Risoluzione  
  
*Utilizzare Gestione commutatori virtuali per visualizzare ed estendere l'intervallo di indirizzi dinamici.*  
  


