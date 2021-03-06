---
title: La replica è sospesa per uno o più macchine virtuali su questo server
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 6974016dd564cf19d39a6d83b041020a4e327d2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861814"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>La replica è sospesa per uno o più macchine virtuali su questo server

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*La replica è stata sospesa per una o più macchine virtuali. Quando la macchina virtuale primaria viene sospesa, tutte le modifiche che si verificano verranno accumulate e verranno inviate alla macchina virtuale di replica dopo la ripresa della replica.*  
  
## <a name="impact"></a>Impatto  
*Fino a quando la replica viene sospesa, le modifiche accumulate che si verificano nella macchina virtuale primaria utilizzeranno lo spazio su disco disponibile nel server primario. Dopo che la replica è stata ripresa, potrebbe essere presente una grande quantità di traffico di rete per il server di replica. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Verificare che sia prevista la replica di sospensione. Se la replica è stata sospesa per risolvere lo spazio su disco insufficiente o la connettività di rete, riprendere la replica non appena tali problemi vengono risolti.*  
  


