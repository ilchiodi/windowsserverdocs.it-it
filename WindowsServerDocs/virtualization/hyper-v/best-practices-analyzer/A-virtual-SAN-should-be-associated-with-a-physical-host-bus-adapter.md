---
title: Una SAN virtuale deve essere associata a una scheda bus host fisico
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3b9ca1e2da1cf9f4410f465fe95c6cc9c0b07ffc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819082"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Una SAN virtuale deve essere associata a una scheda bus host fisico

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
*È stata configurata una rete virtuale archiviazione (SAN) senza un'associazione a una scheda bus host (HBA).*  
  
## <a name="impact"></a>**Impact**  
*Una macchina virtuale avrà esito negativo per l'avvio quando viene configurato con una scheda Fibre Channel virtuale connessa a una SAN virtuale configurato in modo errata. Questo influisce sulle seguenti virtuale SAN:*  
  
  
\<elenco di reti SAN virtuali >  
  
  
## <a name="resolution"></a>**Soluzione**  
*Riconfigurare la SAN virtuale connettendola a una scheda bus host.*  
  
  
  


