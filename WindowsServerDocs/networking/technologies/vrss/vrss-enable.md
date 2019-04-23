---
title: Abilitare vRSS su una scheda di rete virtuale
description: In questo argomento descrive come abilitare vRSS in Windows Server usando Windows PowerShell o Gestione dispositivi.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 19e8011fb98b84c20e8237792664551d2362d589
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882682"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>Abilitare vRSS su una scheda di rete virtuale

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Virtual RSS \(vRSS\) richiede che coda macchine virtuali \(VMQ\) supportano dalla scheda fisica. Se tale funzionalità è disabilitata o non è possibile quindi Receive-side scaling virtuale è disabilitato. 

Per altre informazioni, vedere [prevede l'uso di vRSS](vrss-plan.md).

## <a name="enable-vrss-on-a-vm"></a>Abilitare vRSS in una macchina virtuale
 
Usare le procedure seguenti per abilitare vRSS tramite Windows PowerShell o gestione dei dispositivi.

-   Gestione dispositivi
-   Windows PowerShell
  
### <a name="device-manager"></a>Gestione dispositivi

È possibile utilizzare questa procedura per abilitare vRSS tramite Gestione dispositivi.

>[!NOTE]
>Il primo passaggio in questa procedura è specifico per le macchine virtuali che eseguono Windows 10 o Windows Server 2016. Se la macchina virtuale è in esecuzione un sistema operativo diverso, è possibile aprire Gestione dispositivi prima apertura del Pannello di controllo, quindi individuando e aprendo Gestione dispositivi.
  
1.  Sulla barra delle applicazioni di VM, in **digitare qui per cercare**, digitare **dispositivo**. 

2.  Nei risultati della ricerca, fare clic su **Gestione dispositivi**.

3.  In Gestione dispositivi fare clic per espandere **schede di rete**. 

4.  Fare doppio clic su scheda di rete si desidera configurare e quindi fare clic su **proprietà**.<p>La scheda di rete **proprietà** verrà visualizzata la finestra di dialogo.

5.  Nella scheda di rete **delle proprietà**, fare clic sui **avanzate** scheda. 

6.  Nelle **proprietà**, scorrere verso il basso e fare clic su **Receive-side scaling**. 

7.  Assicurarsi che la selezione nella **valore** viene **abilitato**. 

8.  Fare clic su **OK**.
  
> [!NOTE]
> Nel **avanzate** scheda, alcune schede di rete vengono visualizzano il numero di code RSS supportati dall'adapter.

---

### <a name="windows-powershell"></a>Windows PowerShell

Utilizzare la procedura seguente per abilitare vRSS tramite Windows PowerShell.

1. Nella macchina virtuale, aprire **Windows PowerShell**.

2. Digitare il comando seguente, assicurandosi di sostituire il *AdapterName* valore per il **-nome** parametro con il nome della scheda di rete che si desidera configurare e quindi premere INVIO. 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >In alternativa, è possibile usare il comando seguente per abilitare vRSS.
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

Per altre informazioni, vedere [i comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

---