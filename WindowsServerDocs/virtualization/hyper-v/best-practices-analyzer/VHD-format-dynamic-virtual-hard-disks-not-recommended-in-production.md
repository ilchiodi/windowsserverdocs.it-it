---
title: Formato VHD dischi rigidi virtuali dinamici non sono consigliati per le macchine virtuali che eseguono carichi di lavoro server in un ambiente di produzione
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 324a60a0-1d15-4ef2-9f17-23cbd2eb42ce
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 195bc4c85d380c8f0b15b27d042b30491f635d5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854174"
---
# <a name="vhd-format-dynamic-virtual-hard-disks-are-not-recommended-for-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Formato VHD dischi rigidi virtuali dinamici non sono consigliati per le macchine virtuali che eseguono carichi di lavoro server in un ambiente di produzione

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
*Una o più macchine virtuali utilizzano dischi rigidi virtuali a espansione dinamica in formato VHD.*  
  
## <a name="impact"></a>**Impatto**  
*I dischi rigidi virtuali dinamici in formato VHD potrebbero riscontrare problemi di coerenza se si verifica un errore di alimentazione. Possono verificarsi problemi di coerenza se il disco fisico esegue un aggiornamento incompleto o non corretto di un settore in un file con estensione VHD che viene modificato quando si verifica un errore di alimentazione. Ciò influiscono sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Arrestare la macchina virtuale e convertire il disco rigido virtuale dinamico in formato VHD in un disco rigido virtuale in formato VHDX o in un disco rigido virtuale fisso. Il formato VHDX ha meccanismi di affidabilità che consentono di proteggere il disco da danneggiamenti a causa di problemi di alimentazione del sistema. Tuttavia, non convertire il disco rigido virtuale se è probabile che sia collegato a una versione precedente di Windows in un determinato momento. Le versioni di Windows precedenti a Windows Server 2012 non supportano il formato VHDX.*  
  


