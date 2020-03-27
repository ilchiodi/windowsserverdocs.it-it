---
title: Abilitare vRSS in una scheda di rete virtuale
description: In questo argomento viene illustrato come abilitare vRSS in Windows Server utilizzando Device Manager o Windows PowerShell.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cb48315c-0204-4927-aa24-64f6789c2e20
manager: dougkim
ms.localizationpriority: medium
ms.date: 09/05/2018
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e4be9060da4a738e3ad8e4976d037f3a05467da3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315370"
---
# <a name="enable-vrss-on-a-virtual-network-adapter"></a>Abilitare vRSS in una scheda di rete virtuale

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

RSS virtuale \(vRSS\) richiede Coda macchine virtuali \(supporto\) VMQ dalla scheda fisica. Se VMQ è disabilitato o non è supportato, Receive-Side Scaling virtuale è disabilitato. 

Per altre informazioni, vedere [pianificare l'uso di vRSS](vrss-plan.md).

## <a name="enable-vrss-on-a-vm"></a>Abilitare vRSS in una macchina virtuale
 
Usare le procedure seguenti per abilitare vRSS usando Windows PowerShell o Device Manager.

-   Gestione dispositivi
-   Windows PowerShell
  
### <a name="device-manager"></a>Gestione dispositivi

È possibile usare questa procedura per abilitare vRSS usando Device Manager.

>[!NOTE]
>Il primo passaggio di questa procedura è specifico per le macchine virtuali che eseguono Windows 10 o Windows Server 2016. Se nella macchina virtuale è in esecuzione un sistema operativo diverso, è possibile aprire Device Manager aprendo prima il pannello di controllo e quindi individuando e aprendo Device Manager.
  
1.  Nella barra delle applicazioni della VM, digitare **qui per cercare**, digitare **Device**. 

2.  Nei risultati della ricerca fare clic su **Device Manager**.

3.  In Device Manager fare clic per espandere **schede di rete**. 

4.  Fare clic con il pulsante destro del mouse sulla scheda di rete che si desidera configurare, quindi scegliere **Proprietà**.<p>Verrà visualizzata la finestra di dialogo **Proprietà** scheda di rete.

5.  In **Proprietà**scheda di rete fare clic sulla scheda **Avanzate** . 

6.  In **Proprietà**scorrere verso il basso e fare clic su **Receive-Side Scaling**. 

7.  Verificare che la selezione in **valore** sia **abilitata**. 

8.  Fare clic su **OK**.
  
> [!NOTE]
> Nella scheda **Avanzate** alcune schede di rete visualizzano anche il numero di code RSS supportate dall'adapter.

---

### <a name="windows-powershell"></a>Windows PowerShell

Usare la procedura seguente per abilitare vRSS usando Windows PowerShell.

1. Nella macchina virtuale aprire **Windows PowerShell**.

2. Digitare il comando seguente, assicurandosi di sostituire il valore di *AdapterName* per il parametro **-Name** con il nome della scheda di rete che si desidera configurare, quindi premere INVIO. 
  
   ```PowerShell
   Enable-NetAdapterRSS -Name "AdapterName"
   ```

   >[!TIP]
   >In alternativa, è possibile usare il comando seguente per abilitare vRSS.
   >```PowerShell
   >Set-NetAdapterRSS -Name "AdapterName" -Enabled $True  
   >```

Per ulteriori informazioni, vedere [comandi di Windows PowerShell per RSS e vRSS](vrss-wps.md).

---