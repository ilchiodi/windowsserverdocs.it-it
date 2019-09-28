---
title: Evitare di abilitare le macchine virtuali configurate con gli adapter Fibre Channel virtuali per consentire le migrazioni in tempo reale quando sono presenti meno percorsi per Fibre Channel unità logiche (lun) nella destinazione rispetto all'origine
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c55a8c76391ae1b01f43492dc5c72e3760371b80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365282"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>Evitare di abilitare le macchine virtuali configurate con gli adapter Fibre Channel virtuali per consentire le migrazioni in tempo reale quando sono presenti meno percorsi per Fibre Channel unità logiche (lun) nella destinazione rispetto all'origine

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>**Problema**  
*Una o più macchine virtuali hanno la proprietà AllowReducedFcRedunancy impostata nel provider WMI di virtualizzazione.*  
  
## <a name="impact"></a>**Impatto**  
*La migrazione in tempo reale delle macchine virtuali seguenti potrebbe causare la perdita di dati o l'I/O di interrupt per l'archiviazione:*  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
@no__t 0Consider la cancellazione della proprietà WMI AllowReducedFcRedundancy nelle macchine virtuali interessate. Quando questa proprietà è deselezionata, è possibile eseguire una migrazione in tempo reale su macchine virtuali configurate con adapter Fibre Channel virtuali solo quando il numero di percorsi da Fibre Channel nella destinazione è uguale o superiore al numero di percorsi nell'origine. Questi controlli consentono di evitare la perdita di dati o l'interruzione dell'I/O nella risorsa di archiviazione. * 