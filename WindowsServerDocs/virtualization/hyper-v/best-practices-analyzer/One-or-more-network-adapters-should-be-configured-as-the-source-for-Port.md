---
title: Uno o più schede di rete devono essere configurate come origine per il Mirroring delle porte
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 147fd00f-1440-44d1-94e3-3a8af63aa7ed
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: a079033608b8af8c63a0d02ae166eb7280fec41d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393569"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-source-for-port-mirroring"></a>Uno o più schede di rete devono essere configurate come origine per il Mirroring delle porte

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
*Una o più macchine virtuali dispongono di una scheda di rete configurata come destinazione per il mirroring delle porte, ma non esiste alcuna origine corrispondente nel commutire virtuale.*  
  
## <a name="impact"></a>**Impatto**  
*Il mirroring delle porte non funzionerà correttamente per i commutatori virtuali e le macchine virtuali seguenti:*  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Usare Windows PowerShell o la console di gestione di Hyper-V per completare o correggere la configurazione per il mirroring delle porte.*  
  


