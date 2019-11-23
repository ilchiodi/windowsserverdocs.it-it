---
title: Un team associato a uno switch virtuale può contenere solo un'interfaccia esposta team
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6baa9e4ae900c9b671003872b4eb4589efb2f085
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365397"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>Un team associato a uno switch virtuale può contenere solo un'interfaccia esposta team

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Uno o più commutatori virtuali sono associati a un team con più interfacce del team.*  
  
## <a name="impact"></a>**Impatto**  
*I commutatori virtuali seguenti potrebbero non avere accesso alle VLAN e alla larghezza di banda utilizzata da altre interfacce del team:*  
  
\<elenco di commutatori virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Usare il cmdlet Remove-NetLbfoTeamNic di Windows PowerShell per rimuovere tutte le interfacce del team dal team diverso dall'interfaccia del team predefinita.*  
  


