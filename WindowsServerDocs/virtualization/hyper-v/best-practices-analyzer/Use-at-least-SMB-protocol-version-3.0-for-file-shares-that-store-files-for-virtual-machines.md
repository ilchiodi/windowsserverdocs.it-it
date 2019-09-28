---
title: Utilizzare almeno versione 3.0 del protocollo SMB per le condivisioni di file da archiviare i file per le macchine virtuali.
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4bb832b8-f1aa-4c1f-a0f2-324dd53553ea
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: af23c3c860a47d0dd9096bc3f5ff466aca7836b6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393317"
---
# <a name="use-at-least-smb-protocol-version-30-for-file-shares-that-store-files-for-virtual-machines"></a>Utilizzare almeno versione 3.0 del protocollo SMB per le condivisioni di file da archiviare i file per le macchine virtuali.

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
*I file della macchina virtuale o i file del disco rigido virtuale sono archiviati in una condivisione file che non supporta almeno la versione 3,0 del protocollo SMB.*  
  
## <a name="impact"></a>**Impatto**  
*Microsoft non supporta questa configurazione. Ciò influisca sulle macchine virtuali seguenti:*  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Spostare i file in una condivisione file che utilizza almeno la versione 3,0 del protocollo SMB.*  
  


