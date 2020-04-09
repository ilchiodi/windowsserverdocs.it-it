---
title: Utilizzare almeno versione 3.0 del protocollo SMB per le condivisioni di file da archiviare i file per le macchine virtuali.
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4bb832b8-f1aa-4c1f-a0f2-324dd53553ea
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 64c78c4725c84cbd02276cb2c0eadffae42d7060
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854194"
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
*Questa configurazione non è supportata da Microsoft. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Spostare i file in una condivisione file che utilizza almeno la versione 3,0 del protocollo SMB.*  
  


