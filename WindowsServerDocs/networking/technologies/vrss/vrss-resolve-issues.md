---
title: Risolvere problemi di vRSS
description: Risolvere i problemi di vRSS se non viene visualizzato il traffico di bilanciamento del carico vRSS per la macchina virtuale LPs.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/04/2018
ms.openlocfilehash: c214b0f7ffdceb662b783b0a3603e65604243261
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315270"
---
# <a name="resolve-vrss-issues"></a>Risolvere problemi di vRSS

Se sono stati completati tutti i passaggi di preparazione e non viene ancora visualizzato il traffico di bilanciamento del carico vRSS per la macchina virtuale LPs, esistono diversi possibili problemi.

1. Prima di eseguire i passaggi di preparazione, vRSS è stato disabilitato e ora deve essere abilitato. È possibile eseguire **set-VMNetworkAdapter** per abilitare vRSS per la macchina virtuale.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS è stato disabilitato nella macchina virtuale o nell'host vNIC. Windows Server 2016 Abilita RSS per impostazione predefinita; qualcuno potrebbe averlo disabilitato. 

   - Enabled = **true**

   **Visualizzare le impostazioni correnti:** 

   Eseguire il cmdlet di PowerShell seguente nella macchina virtuale\(per vRSS in una VM\) o nel \(host per l'host vNIC vRSS\).

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Abilitare la funzionalità:** 

   Per modificare il valore da false a true, eseguire il cmdlet di PowerShell seguente.

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   Un altro modo a livello di sistema per configurare RSS consiste nell'usare netsh. Utilizzo 
   
    ```cmd
   netsh int tcp show global
   ```
   
   per assicurarsi che RSS non sia disabilitato a livello globale. E abilitarla, se necessario. Questa impostazione non viene toccata da *-NetAdapterRSS.

3. Se VMMQ non è abilitato dopo la configurazione di vRSS, verificare le impostazioni seguenti in ogni scheda collegata al Commuter virtuale:

   - VmmqEnabled = **false**
   - VmmqEnabledRequested = **true**

   ![Abilitazione di vmmq](../../media/vmmq-enabled.png)

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Abilitare la funzionalità:** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_ Non è possibile abilitare VMMQ (VmmqEnabled = false) quando si imposta **VrssQueueSchedulingMode** su **Dynamic**. Il VrssQueueSchedulingMode non passa a Dynamic dopo l'abilitazione di VMMQ.<p>Il **VrssQueueSchedulingMode** di **Dynamic** richiede supporto driver quando VMMQ è abilitato.  VMMQ è un offload del posizionamento dei pacchetti sui processori logici e, di conseguenza, richiede il supporto del driver per sfruttare l'algoritmo dinamico.  Installare il driver e il firmware del fornitore NIC che supporta VMMQ dinamici.



---
