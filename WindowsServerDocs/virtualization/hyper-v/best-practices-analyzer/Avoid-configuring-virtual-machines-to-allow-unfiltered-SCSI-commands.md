---
title: Evitare di configurare le macchine virtuali per consentire i comandi SCSI non filtrati
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ac059bce1704a4e72b2c373d8186dbd4e31f2164
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857794"
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
  
*Il bypass del filtro del comando SCSI costituisce un rischio per la sicurezza. Questa configurazione deve essere abilitata solo se è necessaria per la compatibilità con le applicazioni di archiviazione in esecuzione nel sistema operativo guest. Le macchine virtuali seguenti sono configurate per consentire i comandi SCSI non filtrati:*  
  
\<elenco dei nomi delle macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Contattare il fornitore del sistema di archiviazione per determinare se questa configurazione è obbligatoria. Inoltre, se il sistema operativo di gestione o altri sistemi operativi guest sono compromessi o presentano un comportamento insolito, riconfigurare la macchina virtuale per bloccare i comandi.*  
  


