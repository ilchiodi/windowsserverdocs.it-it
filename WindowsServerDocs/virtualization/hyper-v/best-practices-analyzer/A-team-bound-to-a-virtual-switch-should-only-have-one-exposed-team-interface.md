---
title: Un team associato a uno switch virtuale può contenere solo un'interfaccia esposta team
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 108bbec1439959bb7ab4475b59c7231653952ea8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838462"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>Un team associato a uno switch virtuale può contenere solo un'interfaccia esposta team

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
*Uno o più commutatori virtuali sono associate a un team che ha più interfacce di team.*  
  
## <a name="impact"></a>**Impact**  
*I commutatori virtuali seguenti potrebbero non avere accesso a reti VLAN e larghezza di banda utilizzata da altre interfacce team:*  
  
\<elenco di commutatori virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Usare il cmdlet di Windows PowerShell Remove-NetLbfoTeamNic per rimuovere tutte le interfacce di team da team diversi da interfaccia team predefinita.*  
  


