---
title: Evitare di eseguire il mapping di un percorso di archiviazione a più pool di risorse
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 53ef04a2dde875b26dd109075a2cfa4484ebd5cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857754"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Evitare di eseguire il mapping di un percorso di archiviazione a più pool di risorse

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>**Problema**  
*Viene eseguito il mapping di un percorso del file di archiviazione a più pool di risorse.*  
  
## <a name="impact"></a>**Impatto**  
*Per il tipo di pool di archiviazione specificato, i pool padre e figlio seguenti condividono lo stesso percorso di archiviazione:*  
  
\<elenco di pool >  
  
## <a name="resolution"></a>**Soluzione**  
*Usare Windows PowerShell per riconfigurare i pool di risorse di archiviazione in modo che più pool non usino lo stesso percorso di archiviazione.*  
  


