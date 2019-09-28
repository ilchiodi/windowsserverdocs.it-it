---
title: I controller di archiviazione devono essere abilitati nelle macchine virtuali per fornire accesso ai dispositivi di archiviazione collegati
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f0d10ab4c419a6014a9edb4b7f721714dc92798d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393488"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>I controller di archiviazione devono essere abilitati nelle macchine virtuali per fornire accesso ai dispositivi di archiviazione collegati

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

## <a name="issue"></a>Problema  
  
*Uno o più controller di archiviazione potrebbero essere disabilitati in una macchina virtuale.*  
  
## <a name="impact"></a>Impatto  
  
i computer @no__t 0Virtual non possono usare l'archiviazione connessa a un controller di archiviazione disabilitato. Ciò influisca sulle macchine virtuali seguenti: *  
  
@no__t 0list di nomi di macchina virtuale >  
  
## <a name="resolution"></a>Risoluzione  
  
*Use Device Manager nel sistema operativo guest per abilitare tutti i controller di archiviazione. Se il controller di archiviazione non è necessario, utilizzare la console di gestione di Hyper-V per rimuoverlo dalla macchina virtuale.*  
  
Per istruzioni su come utilizzare Gestione dispositivi, vedere la Guida del sistema operativo guest. Per istruzioni su come rimuovere il controller di archiviazione, vedere la procedura seguente.  
  
#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>Per rimuovere un controller di archiviazione SCSI dalla macchina virtuale  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **gestione di Hyper-V**.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, selezionare la macchina virtuale che si desidera configurare.  
  
3.  Se la macchina virtuale è in esecuzione, arrestare la macchina virtuale. Fare clic sulla macchina virtuale e fare clic su **arrestare**.  
  
4.  Nel riquadro **Azioni** sotto il nome della macchina virtuale fare clic su **Impostazioni**.  
  
5.  Nel riquadro a sinistra di **impostazioni** nella finestra di dialogo **Hardware**, fare clic su **Controller SCSI**.  
  
6.  Nel riquadro di destra, fare clic su **rimuovere**.  
  


