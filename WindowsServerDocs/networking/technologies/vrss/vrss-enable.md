---
title: Abilitare vRSS in una scheda di rete virtuale
description: In questo argomento, informazioni su come abilitare vRSS in Windows Server tramite Gestione dispositivi o Windows PowerShell.
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133417"
---
# Abilitare vRSS in una scheda di rete virtuale

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Virtuale RSS \(vRSS\) richiede \(VMQ\) coda di macchina virtuale supportano dalla scheda fisica. Se VMQ è disabilitata o non è supportata è disabilitato Virtual Receive-side di ridimensionamento. 

Per altre informazioni, vedi [prevede l'uso di vRSS](vrss-plan.md).

## Abilitare vRSS in una macchina virtuale
 
Utilizzare le procedure seguenti per abilitare vRSS tramite Windows PowerShell o Gestione dispositivi.

-   Gestione dispositivi
-   Windows PowerShell
  
### Gestione dispositivi

È possibile utilizzare questa procedura per abilitare vRSS tramite Gestione dispositivi.

>[!NOTE]
>Il primo passaggio per questa procedura è specifico per le macchine virtuali che eseguono Windows 10 o Windows Server 2016. Se la macchina virtuale è in esecuzione un sistema operativo diverso, puoi aprire Gestione dispositivi prima aprendo il pannello di controllo, quindi individuare e gestione dispositivi.
  
1.  Nella barra delle applicazioni della macchina virtuale, nel **tipo di ricerca**, tipo di **dispositivo**. 

2.  Nei risultati della ricerca, fai clic su **Gestione dispositivi**.

3.  In Gestione dispositivi, fare clic per espandere **le schede di rete**. 

4.  Fai la scheda di rete che desideri configurare e quindi fai clic su **proprietà**.<p>Verrà aperta la finestra di dialogo di **proprietà** di scheda di rete.

5.  Nella scheda di rete **le proprietà**, fare clic sulla scheda **Avanzate** . 

6.  Nella **proprietà**, scorrere verso il basso e fai clic su **Receive-side di ridimensionamento**. 

7.  Assicurati che la selezione nel **valore** sia **abilitato**. 

8.  Fai clic su **OK**.
  
> [!NOTE]
> Nella scheda **Avanzate** , alcune schede di rete anche visualizzano il numero di code RSS supportate dalla scheda di rete.

---

### Windows PowerShell

Usa la procedura seguente per abilitare vRSS tramite Windows PowerShell.

1. Nella macchina virtuale, Apri **Windows PowerShell**.

2. Digita il comando seguente, garantendo che sostituire il valore *AdapterName* per il **-nome** parametro con il nome della scheda di rete che vuoi configurare e quindi premi INVIO. 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >In alternativa, è possibile utilizzare il comando seguente per abilitare vRSS.
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

Per altre informazioni, vedi [I comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

---