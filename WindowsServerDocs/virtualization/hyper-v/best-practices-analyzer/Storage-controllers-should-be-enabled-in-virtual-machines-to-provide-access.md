---
title: I controller di archiviazione devono essere abilitati nelle macchine virtuali per fornire accesso ai dispositivi di archiviazione collegati
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: b530a5868633e6007f311f3d15c94b7ec4ded52c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858794"
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
  
*Le macchine virtuali non possono usare l'archiviazione connessa a un controller di archiviazione disabilitato. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco dei nomi delle macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Usare Device Manager nel sistema operativo guest per abilitare tutti i controller di archiviazione. Se il controller di archiviazione non è necessario, utilizzare Hyper-V Manager per rimuoverlo dalla macchina virtuale.*  
  
Per istruzioni su come utilizzare Gestione dispositivi, vedere la Guida del sistema operativo guest. Per istruzioni su come rimuovere il controller di archiviazione, vedere la procedura seguente.  
  
#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>Per rimuovere un controller di archiviazione SCSI dalla macchina virtuale  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi **Console di gestione di Hyper-V**.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, selezionare la macchina virtuale che si desidera configurare.  
  
3.  Se la macchina virtuale è in esecuzione, arrestare la macchina virtuale. Fare clic sulla macchina virtuale e fare clic su **arrestare**.  
  
4.  Nel riquadro **Azioni** sotto il nome della macchina virtuale fare clic su **Impostazioni**.  
  
5.  Nel riquadro a sinistra di **impostazioni** nella finestra di dialogo **Hardware**, fare clic su **Controller SCSI**.  
  
6.  Nel riquadro di destra, fare clic su **rimuovere**.  
  


