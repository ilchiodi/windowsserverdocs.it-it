---
title: Scegliere tra i checkpoint standard o di produzione in Hyper-V
description: Fornisce istruzioni per configurare una macchina virtuale per usare i checkpoint standard o di produzione
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92bb573b-03b7-470e-b72e-e35edf52b349
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 3591e17c9485fc8f9e365f6322c4f48e783db8ce
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442236"
---
# <a name="choose-between-standard-or-production-checkpoints-in-hyper-v"></a>Scegliere tra i checkpoint standard o di produzione in Hyper-V

>Si applica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

  
A partire da Windows 10 e Windows Server 2016, è possibile scegliere tra i checkpoint standard e di produzione per ogni macchina virtuale. I checkpoint di produzione sono il valore predefinito per nuove macchine virtuali.
  
- I checkpoint di produzione sono "punto nel tempo" immagini di una macchina virtuale, che possono essere ripristinati in un secondo momento in modo che è completamente supportato per tutti i carichi di lavoro. Ciò si ottene creando il checkpoint mediante l'impiego della tecnologia del backup, anziché della tecnologia dello stato salvato, all'interno del sistema operativo guest  
  
- I checkpoint standard acquisire la configurazione hardware, dati e lo stato di una macchina virtuale in esecuzione e devono essere utilizzati in scenari di sviluppo e test. I checkpoint standard possono essere utili se è necessario ricreare un stato specifico o una condizione di una macchina virtuale in esecuzione in modo che è possibile risolvere un problema.  
 
  ## <a name="change-checkpoints-to-production-or-standard-checkpoints"></a>Modificare i checkpoint in produzione o checkpoint standard  
  
1.  In **di gestione di Hyper-V**, fare clic sulla macchina virtuale e fare clic su **impostazioni**.  
  
2.  Sotto il **Management** selezionare **Checkpoint**.  
  
3.  Selezionare i checkpoint di produzione o standard.  
  
    Se si sceglie di checkpoint di produzione, è possibile specificare se l'host deve avere un checkpoint standard se non è possibile eseguire un checkpoint di produzione. Se si deseleziona questa casella di controllo e non può essere creato un checkpoint di produzione, non viene eseguita alcuna operazione di checkpoint.  
  
4.  Se si desidera archiviare i file di configurazione del punto di arresto in un punto diverso, modificarlo nel **percorso del File di Checkpoint** sezione.  
  
5.  Fare clic su **Applica** per salvare le modifiche. Se è stata completata, fare clic su **OK** per chiudere la finestra di dialogo.  
  
> [!NOTE]
> Solo **checkpoint di produzione** sono supportati nei guest che eseguono il ruolo di servizi di dominio Active Directory (Controller di dominio) o di Active Directory Lightweight Directory Services.

## <a name="see-also"></a>Vedere anche  
  
-   [Punti di controllo di produzione](../What-s-new-in-Hyper-V-on-Windows.md#BKMK_check)  
  
-   [Abilitare o disabilitare i checkpoint](Enable-or-disable-checkpoints-in-Hyper-V.md)  
  


