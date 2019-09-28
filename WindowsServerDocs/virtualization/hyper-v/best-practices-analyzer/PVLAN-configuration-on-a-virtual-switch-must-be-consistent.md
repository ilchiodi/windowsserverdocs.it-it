---
title: La configurazione di PVLAN in un Commuter virtuale deve essere coerente
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 36616a5c4d8e57ae929cdab846db65dcdda57b85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364755"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>La configurazione di PVLAN in un Commuter virtuale deve essere coerente

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016| 
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.
  
## <a name="issue"></a>**Problema**  
*La rete locale virtuale privata (PVLAN) non è configurata correttamente in una o più schede di rete virtuali.*  
  
## <a name="impact"></a>**Impatto**  
*PVLAN potrebbe non isolare correttamente il traffico di rete tra le macchine virtuali.*  
  
## <a name="resolution"></a>**Soluzione**  
*Usare il cmdlet di Windows PowerShell, set-VMNetworkAdapterVlan, per configurare PVLAN in modo corretto.*  
  


