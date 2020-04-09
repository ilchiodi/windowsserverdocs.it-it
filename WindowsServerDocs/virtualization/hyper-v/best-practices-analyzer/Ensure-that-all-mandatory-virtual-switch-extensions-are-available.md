---
title: Assicurarsi che siano disponibili tutte le estensioni del commutatore virtuale obbligatorio
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 14c9fc31521a7d4f5e0eed821c0cc49dcfe74e69
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861934"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Assicurarsi che siano disponibili tutte le estensioni del commutatore virtuale obbligatorio

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
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
  


