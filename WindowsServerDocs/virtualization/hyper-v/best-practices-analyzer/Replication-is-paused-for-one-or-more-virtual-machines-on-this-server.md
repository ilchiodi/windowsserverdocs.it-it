---
title: La replica è sospesa per uno o più macchine virtuali su questo server
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 248b5fbdbfb54380e441d14cde6beaa9146ce800
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827772"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>La replica è sospesa per uno o più macchine virtuali su questo server

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*La replica è sospesa per uno o più delle macchine virtuali. Mentre la macchina virtuale primaria è sospeso, tutte le modifiche apportate verranno accumulate e verranno inviate alla macchina virtuale di Replica dopo la replica viene ripresa.*  
  
## <a name="impact"></a>Impatto  
*Fino a quando la replica è sospesa, le modifiche accumulate in corso nella macchina virtuale primaria utilizzerà lo spazio disponibile su disco nel server primario. Dopo la replica viene ripresa, potrebbe esserci un burst di grandi dimensioni di traffico di rete al server di Replica. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Verificare che la replica sospensione era previsto. Se la replica è stata sospesa alla connettività di rete o spazio su disco insufficiente indirizzo, riprendere la replica, non appena vengono risolti i problemi.*  
  


