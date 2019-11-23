---
title: Assicurarsi che siano disponibili tutte le estensioni del commutatore virtuale obbligatorio
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c9363fbce35552a8f7d279662ae9072bcd7ea480
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364819"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Assicurarsi che siano disponibili tutte le estensioni del commutatore virtuale obbligatorio

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Una o più schede di rete virtuali sono connesse a un commutire virtuale con estensioni obbligatorie disabilitate o non installate.*  
  
## <a name="impact"></a>Impatto  
*Il traffico di rete è bloccato su una o più schede di rete virtuali nelle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Prima di tutto, verificare che l'estensione obbligatoria sia stata installata nell'host e installare l'estensione, se necessario. Quindi, se l'estensione obbligatoria è disabilitata, usare gestione Commuter virtuale o il cmdlet di Windows PowerShell Enable-VMSwitchExtension per abilitare l'estensione.*  
  


