---
title: Utilizzare almeno la versione di protocollo SMB 3.0 configurato per la disponibilità continua nelle condivisioni di file che archiviano i file per le macchine virtuali
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3a6cbb6052e2e50b7fd78792c5e01885d7672932
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393336"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>Utilizzare almeno la versione di protocollo SMB 3.0 configurato per la disponibilità continua nelle condivisioni di file che archiviano i file per le macchine virtuali

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
*I file della macchina virtuale o del disco rigido virtuale vengono archiviati in una condivisione di file di rete non configurata con la funzionalità di disponibilità continua della versione 3,0 del protocollo SMB.*  
  
## <a name="impact"></a>**Impatto**  
*Questa configurazione non è consigliata da Microsoft perché potrebbe influisca sulla disponibilità delle macchine virtuali tramite il server. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
Effettua una delle seguenti operazioni:  
  
-   Spostare i file in una condivisione file SMB 3.0 che è configurata per la disponibilità continua.  
  
-   Riconfigurare la condivisione di file corrente per assicurare la disponibilità continua.  
  


