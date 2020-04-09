---
title: Evitare l'uso di dischi rigidi virtuali con una dimensione di settore inferiore alle dimensioni del settore dell'archiviazione fisica in cui è archiviato il file del disco rigido virtuale
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: f7ea02ab83d3d896d2ad3681526133e23725b819
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857704"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>Evitare l'uso di dischi rigidi virtuali con una dimensione di settore inferiore alle dimensioni del settore dell'archiviazione fisica in cui è archiviato il file del disco rigido virtuale

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Operativo** <br />**Sistema**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Uno o più dischi rigidi virtuali hanno dimensioni di settore fisico inferiori alle dimensioni del settore fisico dello spazio di archiviazione in cui si trova il file del disco rigido virtuale.*  
  
## <a name="impact"></a>**Impatto**  
*Potrebbero verificarsi problemi di prestazioni nella macchina virtuale o nell'applicazione che usano il disco rigido virtuale. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Eseguire una delle operazioni seguenti:*  
  
-   *Eseguire una migrazione di archiviazione per spostare il disco rigido virtuale in un nuovo sistema fisico*  
  
-   *Usare Windows PowerShell o WMI per abilitare un disco rigido virtuale in formato VHDX per segnalare una dimensione specifica del settore*  
  
-   *Utilizzare un'impostazione del registro di sistema per abilitare un disco rigido virtuale in formato VHD per segnalare una dimensione del settore fisico di 4K*  
  


