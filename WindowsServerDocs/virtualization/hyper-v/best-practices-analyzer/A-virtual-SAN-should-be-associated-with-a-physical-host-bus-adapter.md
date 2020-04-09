---
title: Una SAN virtuale deve essere associata a una scheda bus host fisico
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 10a17f8661f1436f5b4db317648edde57a0dfe74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857934"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Una SAN virtuale deve essere associata a una scheda bus host fisico

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
*Una rete di archiviazione (SAN) virtuale è stata configurata senza un'associazione a una scheda bus host (HBA).*  
  
## <a name="impact"></a>**Impatto**  
*Una macchina virtuale non verrà avviata quando viene configurata con una scheda di Fibre Channel virtuale connessa a una SAN virtuale non configurata correttamente. Ciò influisca sulle San virtuali seguenti:*  
  
  
\<elenco di San virtuali >  
  
  
## <a name="resolution"></a>**Soluzione**  
*Riconfigurare la SAN virtuale collegandolo a una scheda bus host.*  
  
  
  


