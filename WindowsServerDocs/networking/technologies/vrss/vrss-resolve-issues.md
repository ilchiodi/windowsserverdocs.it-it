---
title: Risolvere problemi di vRSS
description: Risolvere i problemi di vRSS se non viene visualizzato vRSS caricare bilanciamento il traffico verso i processori logici della macchina virtuale.
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
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232520"
---
## Risolvere problemi di vRSS

Se è stato completato tutti i passaggi di preparazione e comunque non viene visualizzato vRSS caricare bilanciamento il traffico verso i processori logici della macchina virtuale, esistono diverse possibili problemi.

1. Prima di operazioni di preparazione è stata eseguita, vRSS è stata disabilitata - e ora deve essere abilitato. È possibile eseguire **Set-VMNetworkAdapter** per abilitare vRSS per la macchina virtuale.

   ```PowerShell
   Set-VMNetworkAdapter <VMname> -VrssEnabled $TRUE
   Set-VMNetworkAdapter -ManagementOS -VrssEnabled $TRUE
   ```

2. RSS è stata disabilitata nella macchina virtuale o in vNIC host. Windows Server 2016 consente RSS per impostazione predefinita. un utente malintenzionato potrebbe avere disattivata. 

   - Abilitato = **True**

   **Visualizzare le impostazioni correnti:** 

   Eseguire il seguente cmdlet PowerShell nel VM\ (per vRSS in un VM\) o nell'host \ (per vRSS\ vNIC host).

   ```PowerShell
   Get-NetAdapterRss
   ```

   **Abilitare la funzionalità:** 

   Per modificare il valore False su True, Esegui il seguente cmdlet PowerShell.

   ```PowerShell
   Enable-NetAdapterRss *
   ```
   
   Un altro modo a livello di sistema per configurare RSS Usa netsh. Usare 
   
    ```cmd
   netsh int tcp show global
   ```
   
   per assicurarti che tale RSS non è disabilitato a livello globale. E abilitarlo se necessario. Questa impostazione non ha toccata dal *-NetAdapterRSS.

3. Se ritieni che VMMQ non è abilitata dopo aver configurato vRSS, verificare le impostazioni seguenti per ogni scheda associata al commutatore virtuale:

   - VmmqEnabled = **False**
   - VmmqEnabledRequested = **True**

   ![abilitato vmmq](../../media/vmmq-enabled.png)

   **Visualizzare le impostazioni correnti:** 

   ```PowerShell
   Get-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS'
   ```

   **Abilitare la funzionalità:** 

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name NICName -DisplayName 'Virtual Switch RSS' -DisplayValue Enabled”
   ```
 
4. _(Windows Server 2019)_ Non è possibile abilitare VMMQ (VmmqEnabled = False) durante l'impostazione **VrssQueueSchedulingMode** in **dinamico**. Il VrssQueueSchedulingMode non cambia in dinamico dopo aver abilitato VMMQ.<p>Il **VrssQueueSchedulingMode** di **Dynamic** richiede il supporto dei driver quando VMMQ è abilitato.  VMMQ è un offload del posizionamento dei pacchetti su processori logici e di conseguenza, è necessario il supporto di driver per sfruttare l'algoritmo dinamico.  Installare driver e firmware che supporta VMMQ dinamica del fornitore scheda di rete.



---
