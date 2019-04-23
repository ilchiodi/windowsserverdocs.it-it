---
title: Evitare di abilitare le macchine virtuali configurate con schede Fibre Channel virtuale per consentire migrazioni in tempo reale quando sono presenti percorsi un minor numero di unità logiche di Fibre Channel (LUN) nella destinazione rispetto all'origine
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6ff69d5cb09133a806c2a2df3446713264a4e892
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849552"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>Evitare di abilitare le macchine virtuali configurate con schede Fibre Channel virtuale per consentire migrazioni in tempo reale quando sono presenti percorsi un minor numero di unità logiche di Fibre Channel (LUN) nella destinazione rispetto all'origine

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>**Problema**  
*Uno o più macchine virtuali hanno la proprietà AllowReducedFcRedunancy impostata nel provider WMI di virtualizzazione.*  
  
## <a name="impact"></a>**Impact**  
*Migrazione in tempo reale delle macchine virtuali seguenti potrebbe causare la perdita di dati o interrompere i/o all'archiviazione:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*È consigliabile deselezionare la proprietà WMI AllowReducedFcRedundancy nelle macchine virtuali interessate. Quando questa proprietà è deselezionata, è possibile eseguire una migrazione in tempo reale su macchine virtuali configurate con schede Fibre Channel virtuali solo quando il numero di percorsi a Fibre Channel nella destinazione è uguale o maggiore del numero di percorsi nell'origine. Questi controlli aiutare a evitare la perdita di dati o di un'interruzione dei / o nella risorsa di archiviazione.* 