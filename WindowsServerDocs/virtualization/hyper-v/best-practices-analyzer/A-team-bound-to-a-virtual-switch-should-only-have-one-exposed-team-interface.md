---
title: Un team associato a uno switch virtuale può contenere solo un'interfaccia esposta team
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 413448945d2598ba36bed646144a43e39a1a3159
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857944"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>Un team associato a uno switch virtuale può contenere solo un'interfaccia esposta team

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema
*Uno o più commutatori virtuali sono associati a un team con più interfacce del team.*  
  
## <a name="impact"></a>Impatto
*I commutatori virtuali seguenti potrebbero non avere accesso alle VLAN e alla larghezza di banda utilizzata da altre interfacce del team:*  
  
\<elenco di commutatori virtuali >  
  
## <a name="resolution"></a>Risoluzione
*Usare il cmdlet Remove-NetLbfoTeamNic di Windows PowerShell per rimuovere tutte le interfacce del team dal team diverso dall'interfaccia del team predefinita.*  
  


