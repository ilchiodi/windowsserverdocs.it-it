---
title: La configurazione di PVLAN in un Commuter virtuale deve essere coerente
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 7e42c874e883157b3f85f523511b950e2863b155
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861854"
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
  


