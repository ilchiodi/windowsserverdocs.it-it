---
title: Utilizzo di RAM che fornisce la correzione degli errori
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 67eb6cef-b045-4748-90e1-406af5345d6a
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: cd12125307c99956b9207a1e56c8d5f66a2e6bd2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854224"
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
  


