---
title: Evitare di configurare le macchine virtuali per consentire i comandi SCSI non filtrati
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f401ce4d72f88d72529a95acea2a999df93679b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888272"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>Evitare di configurare le macchine virtuali per consentire i comandi SCSI non filtrati

>Si applica a: Windows Server 2016


  
*Per altre informazioni sulle procedure consigliate e le analisi, vedere* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Operazioni|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Una macchina virtuale è configurata per consentire i comandi SCSI non filtrati.*  
  
## <a name="impact"></a>Impatto  
  
*Ignorando il comando SCSI filtri può causare un rischio per la sicurezza. Questa configurazione deve essere abilitata solo se è necessaria per la compatibilità con le applicazioni di archiviazione in esecuzione nel sistema operativo guest. Le macchine virtuali seguenti siano configurate per consentire i comandi SCSI non filtrati:*  
  
\<elenco di nomi delle macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Contattare il fornitore di archiviazione per determinare se questa configurazione è necessaria. Inoltre, se il sistema operativo di gestione o altri sistemi operativi guest vengono compromesse o presentano un comportamento insolito, riconfigurare la macchina virtuale in modo da bloccare i comandi.*  
  


