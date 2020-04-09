---
title: Il numero di processori logici in uso non deve superare il valore massimo supportato
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: b6cd948c47e58dec919cd946ad701f70403d6af3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859294"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>Il numero di processori logici in uso non deve superare il valore massimo supportato

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Condizione|  
  
Nelle sezioni seguenti, corsivo indica il testo visualizzato nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Il server è configurato con un numero eccessivo di processori logici.*  
  
## <a name="impact"></a>Impatto  
  
*Microsoft non supporta l'esecuzione di Hyper-V in questo computer.*  
  
## <a name="resolution"></a>Risoluzione  
  
*Rimuovere alcuni processori dal computer o usare msconfig per limitare il numero di processori disponibili.*  
  
Vedere le istruzioni seguenti per utilizzare Msconfig. Per ulteriori informazioni sulla rimozione di processori, vedere le istruzioni fornite con il computer o contattare il produttore dell'hardware. Per informazioni dettagliate sulle configurazioni supportate massime per Hyper-V, vedere [pianificare la scalabilità di Hyper-V in Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
### <a name="to-limit-the-number-of-available-processors"></a>Per limitare il numero di processori disponibili  
  
1.  Per visualizzare la configurazione dell'applicazione (Msconfig.exe). A tale scopo, fare clic su **avviare**, tipo **msconfig**, fare doppio clic su di **configurazione di sistema** app desktop e fare clic su **Esegui come amministratore**.  
  
2.  Dalla **avvio** scheda, fare clic su **Opzioni avanzate**.  
  
3.  Selezionare **numero di processori** e quindi selezionare un numero nell'elenco. Fare clic su **OK**.  
  
4.  Riavviare il computer per l'esecuzione usando il nuovo numero di processori.  
  


