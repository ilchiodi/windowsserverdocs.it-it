---
title: abilitare l'estensione del commutatore virtuale Piattaforma filtro Windows se richiesto da estensioni di terze parti
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8aa8a9a5-e3fa-4c9b-8331-ba5a3de22429
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d4cc23ce638f7b5ee95f80de067b4ad5b360d118
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859304"
---
# <a name="the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions"></a>abilitare l'estensione del commutatore virtuale Piattaforma filtro Windows se richiesto da estensioni di terze parti

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Avviso|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>**Problema**  
*L'estensione del Commuter virtuale Windows Filtering Platform (WFP) è disabilitata.*  
  
## <a name="impact"></a>**Impatto**  
*Alcune estensioni del commutatore virtuale di terze parti potrebbero non funzionare correttamente sui commutatori virtuali seguenti:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>**Soluzione**  
*Usare il cmdlet di Windows PowerShell Enable-VMSwitchExtension per abilitare la piattaforma filtro Windows se è richiesta da estensioni di terze parti.*  
  
### <a name="enable-the-windows-filtering-platform-using-windows-powershell"></a>Abilitare la piattaforma filtro Windows con Windows PowerShell  
  
1.  Aprire Windows PowerShell. (Dal desktop fare clic su **Start** e iniziare a digitare **Windows PowerShell**).  
  
2.  Fare doppio clic su **Windows PowerShell** e fare clic su **Esegui come amministratore**.  
  
3.  Eseguire questo comando dopo aver sostituito esterno con il nome del commutatore esterno:  
  
```  
Enable-VMSwitchExtension -VMSwitchName External -Name Microsoft Windows Filtering Platform  
```  
  
## <a name="see-also"></a>Vedi anche  
[Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx)  
  


