---
title: Evitare di configurare le macchine virtuali per consentire i comandi SCSI non filtrati
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5deb20862ed0e359febd4a9b58202d53c85058ca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365266"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>Evitare di configurare le macchine virtuali per consentire i comandi SCSI non filtrati

>Si applica a: Windows Server 2016


  
*Per ulteriori informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Una macchina virtuale è configurata per consentire i comandi SCSI non filtrati.*  
  
## <a name="impact"></a>Impatto  
  
il filtro del comando SCSI *Bypassing costituisce un rischio per la sicurezza. Questa configurazione deve essere abilitata solo se è necessaria per la compatibilità con le applicazioni di archiviazione in esecuzione nel sistema operativo guest. Le macchine virtuali seguenti sono configurate per consentire i comandi SCSI non filtrati:*  
  
@no__t 0list di nomi di macchina virtuale >  
  
## <a name="resolution"></a>Risoluzione  
  
@no__t: 0Contact il fornitore del sistema di archiviazione per determinare se questa configurazione è necessaria. Inoltre, se il sistema operativo di gestione o altri sistemi operativi guest sono compromessi o presentano un comportamento insolito, riconfigurare la macchina virtuale per bloccare i comandi. *  
  


