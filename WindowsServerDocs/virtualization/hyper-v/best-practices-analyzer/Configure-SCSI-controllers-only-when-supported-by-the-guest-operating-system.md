---
title: Configurare i controller SCSI solo quando è supportata dal sistema operativo guest
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: cf206d9568ef7634d724f3fce450985c34ebfac5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862164"
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
  
*Le macchine virtuali non possono utilizzare l'archiviazione collegata al controller SCSI. Ciò influisca sulle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
  
*Arrestare la macchina virtuale e usare la console di gestione di Hyper-V per rimuovere il controller SCSI dalla macchina virtuale. Quindi, riavviare la macchina virtuale.*  
  


