---
title: Evitare di usare i dischi rigidi virtuali con dimensioni di settore minore delle dimensioni di settore dell'archiviazione fisica che archivia il file di disco rigido virtuale
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b6ec2e0995180ecf9ae5986447fdd460a1c7a337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846262"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>Evitare di usare i dischi rigidi virtuali con dimensioni di settore minore delle dimensioni di settore dell'archiviazione fisica che archivia il file di disco rigido virtuale

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Funzionamento** <br />**System**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*Uno o più dischi rigidi virtuali abbiano dimensioni fisiche di settore inferiori alle dimensioni fisiche di settore dell'archiviazione in cui si trova il file di disco rigido virtuale.*  
  
## <a name="impact"></a>**Impact**  
*Potrebbero verificarsi problemi di prestazioni nella macchina virtuale o applicazioni che utilizzano il disco rigido virtuale. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Eseguire una delle operazioni seguenti:*  
  
-   *Eseguire una migrazione di archiviazione per spostare il disco rigido virtuale in un nuovo sistema fisico*  
  
-   *Usare Windows PowerShell o WMI per abilitare un disco rigido virtuale in formato VHDX per segnalare una dimensione di settore specifico*  
  
-   *Usare un'impostazione del Registro di sistema per abilitare un disco rigido virtuale in formato VHD segnalare una dimensioni fisiche di settore di 4K*  
  


