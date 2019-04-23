---
title: Configurare una macchina virtuale con un controller SCSI in grado di inserimento e scollegare a caldo di archiviazione
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 755e7485e54ee58e0acd7ebd75a7ee591aa655f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843282"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>Configurare una macchina virtuale con un controller SCSI in grado di inserimento e scollegare a caldo di archiviazione

>Si applica a: Windows Server 2016


  
*Per altre informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*È stata trovata una macchina virtuale che non è configurato con un controller SCSI.*  
  
## <a name="impact"></a>Impatto  
  
*Non sarà in grado di hot plug o scollegare a caldo di archiviazione per le macchine virtuali seguenti:*  
  
\<elenco di nomi delle macchine virtuali >  
  
La possibilità di inserimento o scollegare a caldo archiviazione rende più semplice gestire le esigenze di archiviazione di una macchina virtuale senza tempi di inattività. Prima di poter aggiungere o rimuovere spazio di archiviazione, è necessario arrestare le macchine virtuali senza controller SCSI.  
  
## <a name="resolution"></a>Risoluzione  
  
*Se non è necessario hot plug o scollegare a caldo di archiviazione per questa macchina virtuale, è necessaria alcuna azione. In caso contrario, arrestare la macchina virtuale e aggiungere un controller SCSI per la configurazione.*  
  
Per utilizzare un controller SCSI a caldo e a caldo scollegare archiviazione, il sistema operativo guest deve essere in esecuzione la versione corrente di integration services.  
  


