---
title: Configurare i controller SCSI solo quando è supportata dal sistema operativo guest
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: da8d929a8f06f58610913d28d2f1e90299efb235
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366424"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>Configurare i controller SCSI solo quando è supportata dal sistema operativo guest

>Si applica a: Windows Server 2016


  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Una macchina virtuale è configurata con un controller SCSI che non può essere usato perché il sistema operativo guest non supporta i controller SCSI.*  
  
## <a name="impact"></a>Impatto  
  
i computer *Virtual non possono usare l'archiviazione collegata al controller SCSI. Ciò influisca sulle macchine virtuali seguenti:*  
  
@no__t 0list di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
@no__t 0Shut la macchina virtuale e usare la console di gestione di Hyper-V per rimuovere il controller SCSI dalla macchina virtuale. Riavviare quindi la macchina virtuale. *  
  


