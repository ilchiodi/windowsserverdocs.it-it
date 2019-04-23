---
title: Il numero di processori logici in uso non deve superare il valore massimo supportato
description: Fornisce le istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d1275a17cc04494708f5ecfe9b708834b4233641
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847072"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>Il numero di processori logici in uso non deve superare il valore massimo supportato

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Condizione|  
  
Nelle sezioni seguenti, corsivo indica il testo visualizzato nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
  
*Il server è configurato con un numero eccessivo di processori logici.*  
  
## <a name="impact"></a>Impatto  
  
*Microsoft non supporta l'esecuzione di Hyper-V nel computer.*  
  
## <a name="resolution"></a>Risoluzione  
  
*Rimuovere alcuni processori da questo computer o utilizzare msconfig per limitare il numero di processori disponibili.*  
  
Vedere le istruzioni seguenti per utilizzare Msconfig. Per ulteriori informazioni sulla rimozione di processori, vedere le istruzioni fornite con il computer o contattare il produttore dell'hardware. Per informazioni dettagliate sulle configurazioni supportate massime per Hyper-V, vedere [pianificare la scalabilità di Hyper-V in Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
### <a name="to-limit-the-number-of-available-processors"></a>Per limitare il numero di processori disponibili  
  
1.  Per visualizzare la configurazione dell'applicazione (Msconfig.exe). A tale scopo, fare clic su **avviare**, tipo **msconfig**, fare doppio clic su di **configurazione di sistema** app desktop e fare clic su **Esegui come amministratore**.  
  
2.  Dalla **avvio** scheda, fare clic su **Opzioni avanzate**.  
  
3.  Selezionare **numero di processori** e quindi selezionare un numero nell'elenco. Fare clic su **OK**.  
  
4.  Riavviare il computer per l'esecuzione usando il nuovo numero di processori.  
  


