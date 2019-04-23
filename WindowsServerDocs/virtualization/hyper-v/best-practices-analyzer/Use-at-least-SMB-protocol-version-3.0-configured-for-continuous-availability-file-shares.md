---
title: Utilizzare almeno la versione di protocollo SMB 3.0 configurato per la disponibilità continua nelle condivisioni di file che archiviano i file per le macchine virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 67f41293433bd8d14096688fbaa23eb43334c738
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877872"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>Utilizzare almeno la versione di protocollo SMB 3.0 configurato per la disponibilità continua nelle condivisioni di file che archiviano i file per le macchine virtuali

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
*File della macchina virtuale o file disco rigido virtuale vengono archiviati in una condivisione di file di rete che non è configurata con la funzionalità di disponibilità continua di versione del protocollo SMB 3.0.*  
  
## <a name="impact"></a>**Impact**  
*Microsoft sconsiglia questa configurazione perché potrebbe influire sulla disponibilità delle macchine virtuali usando il server. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
Effettua una delle seguenti operazioni:  
  
-   Spostare i file in una condivisione file SMB 3.0 che è configurata per la disponibilità continua.  
  
-   Riconfigurare la condivisione di file corrente per assicurare la disponibilità continua.  
  


