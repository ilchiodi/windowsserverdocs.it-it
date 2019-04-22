---
title: Risolvere problemi di vRSS
description: Risolvere i problemi di vRSS se non viene visualizzata vRSS il carico del traffico a LPs la macchina virtuale.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: a2d6eb43149361b4270565b63fc99f483f364f74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824032"
---
## <a name="resolve-vrss-issues"></a>Risolvere problemi di vRSS

Se è stato completato tutti i passaggi di preparazione e ancora non risulta vRSS il carico del traffico per la macchina virtuale LPs, esistono diversi possibili problemi.

1. Prima di avere eseguito i passaggi di preparazione, vRSS è stata disabilitata - e ora deve essere abilitato. È possibile eseguire **Set-VMNetworkAdapter** abilitare vRSS per le VM.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS è stata disabilitata nella macchina virtuale o in vNIC host. Windows Server 2016 consente RSS per impostazione predefinita. qualcuno potrebbe averla disattivata. 

   - Abilitato = **True**

   **Visualizzare le impostazioni correnti:** 

   Eseguire il seguente cmdlet di PowerShell nella macchina virtuale\(per vRSS in una macchina virtuale\) o nell'host \(per host vNIC vRSS\).

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Abilitare la funzionalità:** 

   Per modificare il valore da False a True, eseguire il cmdlet di PowerShell seguente.

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   Un altro modo a livello di sistema per configurare RSS utilizza netsh. Uso 
   
    ```cmd
   netsh int tcp show global
   ```
   
   Per assicurarsi che tale RSS non è disabilitato a livello globale. E, se necessario riabilitarlo. Questa impostazione non è interessata dalle *-NetAdapterRSS.

3. Se si ritiene che VMMQ non è abilitato dopo aver configurato vRSS, verificare le impostazioni seguenti in ogni scheda collegata al commutatore virtuale:

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![vmmq-enabled](../../media/vmmq-enabled.png)

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Abilitare la funzionalità:** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_  Non è possibile abilitare VMMQ (VmmqEnabled = False) mentre l'impostazione **VrssQueueSchedulingMode** al **dinamico**. Il VrssQueueSchedulingMode non cambia in dinamico dopo aver abilitato VMMQ.<p>Il **VrssQueueSchedulingMode** dei **dinamico** richiede il supporto del driver quando VMMQ è abilitata.  VMMQ è un offload della posizione dei pacchetti su processori logici e di conseguenza, richiede il supporto di driver per sfruttare l'algoritmo dinamico.  Installare driver e firmware che supporta VMMQ dinamica il fornitore di interfaccia di rete.



---
