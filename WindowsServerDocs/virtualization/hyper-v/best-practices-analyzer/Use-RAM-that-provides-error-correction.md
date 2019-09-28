---
title: Utilizzo di RAM che fornisce la correzione degli errori
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 67eb6cef-b045-4748-90e1-406af5345d6a
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c6f232ba44631e35190688d6c48a3bc53224bc17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393450"
---
# <a name="use-ram-that-provides-error-correction"></a>Utilizzo di RAM che fornisce la correzione degli errori

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*La RAM utilizzata in questo computer non è la RAM di correzione degli errori (ECC).*  
  
## <a name="impact"></a>Impatto  
  
*Microsoft non supporta Windows Server 2016 nei computer senza correggere l'errore di RAM.*  
  
## <a name="resolution"></a>Risoluzione  
  
*Verificare che il server sia elencato nel catalogo di Windows Server e che sia qualificato per Hyper-V.*  
  
Per verificare se il server è elencato, vedere il [catalogo di Windows Server](https://www.windowsservercatalog.com/).  
  


