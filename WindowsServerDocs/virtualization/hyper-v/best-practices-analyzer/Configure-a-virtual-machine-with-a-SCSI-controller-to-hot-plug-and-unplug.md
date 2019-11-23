---
title: Configurare una macchina virtuale con un controller SCSI in grado di inserimento e scollegare a caldo di archiviazione
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5b901ee8f11942b8ad50a3c34c53354a5998e105
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365067"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>Configurare una macchina virtuale con un controller SCSI in grado di inserimento e scollegare a caldo di archiviazione

>Si applica a: Windows Server 2016


  
*Per ulteriori informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*È stata rilevata una macchina virtuale non configurata con un controller SCSI.*  
  
## <a name="impact"></a>Impatto  
  
*Non sarà possibile collegare o scollegare a caldo l'archiviazione per le macchine virtuali seguenti:*  
  
\<elenco dei nomi delle macchine virtuali >  
  
La possibilità di inserimento o scollegare a caldo archiviazione rende più semplice gestire le esigenze di archiviazione di una macchina virtuale senza tempi di inattività. Prima di poter aggiungere o rimuovere spazio di archiviazione, è necessario arrestare le macchine virtuali senza controller SCSI.  
  
## <a name="resolution"></a>Risoluzione  
  
*Se non è necessario eseguire il plug-in o scollegare a caldo lo spazio di archiviazione per questa macchina virtuale, non è necessaria alcuna azione. In caso contrario, arrestare la macchina virtuale e aggiungere un controller SCSI alla configurazione.*  
  
Per utilizzare un controller SCSI a caldo e a caldo scollegare archiviazione, il sistema operativo guest deve essere in esecuzione la versione corrente di integration services.  
  


