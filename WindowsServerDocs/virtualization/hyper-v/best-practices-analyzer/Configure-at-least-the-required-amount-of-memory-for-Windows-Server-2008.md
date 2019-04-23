---
title: Configurare almeno la quantità di memoria per una macchina virtuale in esecuzione Windows Server 2008 e abilitato per la memoria dinamica richiesta
description: Versione online del testo per questa regola di Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 0aaa7138-ce83-4b17-add7-fdfa3d469bea
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 17ef3698cdbccd44042e1faa86c0b4eb3a98e135
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876342"
---
# <a name="configure-at-least-the-required-amount-of-memory-for-a-virtual-machine-running-windows-server-2008-and-enabled-for-dynamic-memory"></a>Configurare almeno la quantità di memoria per una macchina virtuale in esecuzione Windows Server 2008 e abilitato per la memoria dinamica richiesta

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**/ Funzionalità del prodotto**|Hyper-V|  
|**Severity**|Errore|  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Uno o più macchine virtuali sono configurate per l'utilizzo della memoria dinamica con minore rispetto alla quantità di memoria necessaria per Windows Server 2008.*  
  
## <a name="impact"></a>Impatto  
*Il sistema operativo guest nelle macchine virtuali seguenti potrebbero non essere eseguiti o potrebbero essere eseguiti unreliably:*  
  
\<elenco di macchine virtuali >  
  
## <a name="resolution"></a>Risoluzione  
*Utilizzare Gestione Hyper-V o Windows PowerShell per aumentare la memoria minima per almeno 256 MB e la memoria di avvio e massima di memoria su almeno 512 MB.*  
  
### <a name="increase-memory-using-hyper-v-manager"></a>Aumentare la memoria tramite Gestione di Hyper-V  
  
1.  Aprire la console di gestione di Hyper-V. (Da Server Manager, fare clic su **Strumenti** > **Hyper-V Manager**.)  
  
2.  Dall'elenco di macchine virtuali, fare doppio clic su quello desiderato, quindi fare clic su **impostazioni**.  
  
3.  Nel riquadro di spostamento, fare clic su **memoria**.  
  
4.  Modifica il **RAM** su almeno 512 MB.  
  
5.  In **la memoria dinamica**,  modificare il **RAM minima** su almeno 256 MB e **RAM massima** a 512 MB.  
  
6.  Fare clic su **OK**.  
  
### <a name="increase-memory-using-windows-powershell"></a>Aumentare la memoria con Windows PowerShell  
  
1.  Aprire Windows PowerShell. (Dal desktop, fare clic su **avviare** e iniziare a digitare **Windows PowerShell**.)  
  
2.  Fare doppio clic su **Windows PowerShell** e fare clic su **Esegui come amministratore**.  
  
3.  Eseguire un comando simile al seguente, sostituendo MyVM con il nome della macchina virtuale e la memoria di valori con almeno i valori riportati di seguito.  
  
```  
Get-VM MyVM | Set-VMMemory -DynamicMemoryEnabled $True -MaximumBytes 512MB -MinimumBytes 256MB -StartupBytes 512MB  
```  
  


