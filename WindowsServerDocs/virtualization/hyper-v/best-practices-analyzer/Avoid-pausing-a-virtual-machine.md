---
title: Evitare la sospensione di una macchina virtuale
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4492ac385a289d075ebcd48b1c7c1c78c1af2f8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814352"
---
# <a name="avoid-pausing-a-virtual-machine"></a>Evitare la sospensione di una macchina virtuale

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

## <a name="issue"></a>Problema  
  
*Questo server ha uno o più macchine virtuali in uno stato di sospensione.*  
  
## <a name="impact"></a>Impatto  
  
*A seconda della quantità di memoria disponibile, potrebbe non essere in grado di eseguire macchine virtuali aggiuntive.*  
  
Le macchine virtuali sospese non rilasciare la memoria allocata, il che significa che la memoria non è disponibile per l'avvio di altre macchine virtuali.  
  
## <a name="resolution"></a>Risoluzione  
  
*Se questa condizione è voluta, non è necessaria alcuna azione ulteriore. In caso contrario, prendere in considerazione la ripresa di queste macchine virtuali o arrestarli.*  
  
#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>Utilizzare Gestione Hyper-V per riprendere la macchina virtuale  
  
1.  Aprire la console di gestione di Hyper-V. (Dal **degli strumenti** dal menu di Server Manager, fare clic su **Hyper-V Manager**.)  
  
2.  Dal **macchine virtuali** elencare, trovare le macchine virtuali con stato **sospeso**.  
  
    > [!IMPORTANT]  
    > Lo stato **Paused-critical** si verifica quando non c'è poco spazio rimanente nello spazio di archiviazione fisica che la macchina virtuale. Prima di tentare di riprendere una macchina virtuale in questo stato, liberare spazio disponibile sull'archiviazione fisica.  
  
3.  Fare doppio clic su ogni nome di macchina virtuale, quindi fare clic su **Riprendi**. Restituisce la macchina virtuale a uno stato di esecuzione. Al termine, se si desidera arrestare la macchina virtuale, anche in questo caso pulsante destro del mouse e scegliere **arrestare**.  
  
#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>Usare Windows PowerShell per riprendere la macchina virtuale  
  
È possibile farlo in un unico comando usando i filtri e la pipeline dopo aver recuperare tutte le macchine virtuali nell'host. Digitare:   
  
```  
get-vm | where state -eq 'paused' | resume-vm  
```  
  


