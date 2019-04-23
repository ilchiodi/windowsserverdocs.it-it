---
title: Utilizzare almeno versione 3.0 del protocollo SMB per le condivisioni di file da archiviare i file per le macchine virtuali.
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4bb832b8-f1aa-4c1f-a0f2-324dd53553ea
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 28e0f3769fd4fc993710d0a0b800dfad7c9ab157
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834342"
---
# <a name="use-at-least-smb-protocol-version-30-for-file-shares-that-store-files-for-virtual-machines"></a>Utilizzare almeno versione 3.0 del protocollo SMB per le condivisioni di file da archiviare i file per le macchine virtuali.

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*File di macchina virtuale o file disco rigido virtuale vengono archiviati in una condivisione di file che non supporta almeno la versione di protocollo SMB 3.0.*  
  
## <a name="impact"></a>**Impact**  
*Microsoft non supporta questa configurazione. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Spostare i file in una condivisione di file che utilizza almeno versione del protocollo SMB 3.0.*  
  


