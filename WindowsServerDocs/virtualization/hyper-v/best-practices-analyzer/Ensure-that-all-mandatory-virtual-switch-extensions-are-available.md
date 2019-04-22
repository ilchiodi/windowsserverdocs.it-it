---
title: Assicurarsi che siano disponibili tutte le estensioni del commutatore virtuale obbligatorio
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 53ceeb9aab6ca7196454fbcd7f0fdae8b34d05d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825942"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Assicurarsi che siano disponibili tutte le estensioni del commutatore virtuale obbligatorio

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Uno o più schede di rete virtuali connessi a un commutatore virtuale con estensioni obbligatori che sono disabilitati o non è installato.*  
  
## <a name="impact"></a>Impatto  
*Il traffico di rete è bloccato su una o più schede di rete virtuale nelle macchine virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*In primo luogo, assicurarsi che l'estensione obbligatorio sia stato installato nell'host e, se necessario, installare l'estensione. Quindi, se l'estensione obbligatorio è disabilitato, utilizzare Gestione commutatori virtuali o i cmdlet di Windows PowerShell Enable-VMSwitchExtension per abilitare l'estensione.*  
  


