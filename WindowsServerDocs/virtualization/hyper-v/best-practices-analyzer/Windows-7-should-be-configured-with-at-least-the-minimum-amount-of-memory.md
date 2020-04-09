---
title: Windows 7 deve essere configurato con almeno la quantità minima di memoria
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer ".
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1b81ec0b-ceca-4fba-83ea-90d5f1d9bda8
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 9f29ff11b601bf3fe841e2634be6096e59d78cd5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854974"
---
# <a name="windows-7-should-be-configured-with-at-least-the-minimum-amount-of-memory"></a>Windows 7 deve essere configurato con almeno la quantità minima di memoria

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e le analisi, vedere [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore|  
|**Categoria**|Configurazione|  

Nelle sezioni seguenti, corsivo indica il testo dell'interfaccia Utente visualizzata nello strumento Analizzatore procedure consigliate per questo problema.

## <a name="issue"></a>Problema  
  
*Una macchina virtuale che esegue Windows 7 è configurata con una quantità di RAM inferiore alla quantità minima di RAM, ovvero 512 MB.*  
  
## <a name="impact"></a>Impatto  
  
*Il sistema operativo guest nelle macchine virtuali seguenti potrebbe non essere eseguito o potrebbe non essere eseguito in modo affidabile:*  
```  
<list of virtual machine names>  
```  
## <a name="resolution"></a>Risoluzione  
  
*Usare la console di gestione di Hyper-V per aumentare la memoria allocata a questa macchina virtuale almeno 512 MB.*  
  
### <a name="to-increase-the-memory-using-hyper-v-manager"></a>Per aumentare la memoria tramite Gestione di Hyper-V  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi **Console di gestione di Hyper-V**.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, selezionare la macchina virtuale che si desidera configurare. Lo stato della macchina virtuale deve essere indicato come **disattivato**. In caso contrario, fare clic con il pulsante destro del mouse sulla macchina virtuale e quindi scegliere **Arresta**.  
  
3.  Nel riquadro **Azioni** sotto il nome della macchina virtuale fare clic su **Impostazioni**.  
  
4.  Nel riquadro di spostamento, fare clic su **memoria**.  
  
5.  Nella pagina **memoria** impostare la RAM di **avvio** su almeno 512 MB e quindi fare clic su **OK**.  
  
### <a name="increase-the-memory-using-windows-powershell"></a>Aumentare la memoria usando Windows PowerShell  
  
1.  Aprire Windows PowerShell. (Dal desktop fare clic su **Start** e iniziare a digitare **Windows PowerShell**).  
  
2.  Fare doppio clic su **Windows PowerShell** e fare clic su **Esegui come amministratore**.  
  
3.  Eseguire questo comando dopo aver sostituito \<MyVM > con il nome della macchina virtuale:  
  
```  
Set-VMMemory <MyVM> -StartupBytes 512MB  
```  
  
## <a name="see-also"></a>Vedi anche  
[Set-VMMemory](https://technet.microsoft.com/library/hh848572.aspx)  
  


