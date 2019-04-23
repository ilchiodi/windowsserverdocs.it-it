---
title: I controller di archiviazione devono essere abilitati nelle macchine virtuali per fornire accesso ai dispositivi di archiviazione collegati
description: Fornisce le istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 42803a0eef84bf006e9f9e7ed6297ea21b4eb7b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849162"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>I controller di archiviazione devono essere abilitati nelle macchine virtuali per fornire accesso ai dispositivi di archiviazione collegati

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

## <a name="issue"></a>Problema  
  
*È possibile disabilitare uno o più controller di archiviazione in una macchina virtuale.*  
  
## <a name="impact"></a>Impatto  
  
*Le macchine virtuali non è possibile usare l'archiviazione connessa a un controller di archiviazione disabilitato. Questo influisce sulle macchine virtuali seguenti:*  
  
\<elenco di nomi delle macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Usare Gestione dispositivi nel sistema operativo guest per abilitare tutti i controller di archiviazione. Se il controller di archiviazione non necessario, utilizzare Hyper-V Manager per rimuoverlo dalla macchina virtuale.*  
  
Per istruzioni su come utilizzare Gestione dispositivi, vedere la Guida del sistema operativo guest. Per istruzioni su come rimuovere il controller di archiviazione, vedere la procedura seguente.  
  
#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>Per rimuovere un controller di archiviazione SCSI dalla macchina virtuale  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **gestione di Hyper-V**.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, selezionare la macchina virtuale che si desidera configurare.  
  
3.  Se la macchina virtuale è in esecuzione, arrestare la macchina virtuale. Fare clic sulla macchina virtuale e fare clic su **arrestare**.  
  
4.  Nel riquadro **Azioni** sotto il nome della macchina virtuale fare clic su **Impostazioni**.  
  
5.  Nel riquadro a sinistra di **impostazioni** nella finestra di dialogo **Hardware**, fare clic su **Controller SCSI**.  
  
6.  Nel riquadro di destra, fare clic su **rimuovere**.  
  


