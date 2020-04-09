---
title: Gestire le macchine virtuali Windows con PowerShell Direct
description: Fornisce istruzioni per l'uso di PowerShell Direct per gestire le macchine virtuali senza basarsi su una rete o su una connessione remota.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: b5715c02-a90f-4de9-a71e-0fc09093ba2d
author: kbdazure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: c4a051de2d8f62c38ae0c44b1a62d5bf9df339e8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859434"
---
# <a name="manage-windows-virtual-machines-with-powershell-direct"></a>Gestire le macchine virtuali Windows con PowerShell Direct

>Si applica a: Windows 10, Windows Server 2016, Windows Server 2019
  
È possibile usare PowerShell Direct per gestire in remoto una macchina virtuale Windows 10, Windows Server 2016 o Windows Server 2019 da un host Hyper-V per Windows 10, Windows Server 2016 o Windows Server 2019. PowerShell Direct consente la gestione di Windows PowerShell all'interno di una macchina virtuale indipendentemente dalla configurazione di rete o dalle impostazioni di gestione remota nell'host Hyper-V o nella macchina virtuale. Questo rende più semplice per gli amministratori di Hyper-V automatizzare e basare su script la gestione e la configurazione delle macchine virtuali.  
  
È possibile eseguire PowerShell Direct in due modi:  
  
- Creare e chiudere una sessione di PowerShell Direct con i cmdlet PSSession
  
- Eseguire uno script o un comando con il cmdlet Invoke-Command
  
Se è necessario gestire macchine virtuali di versioni precedenti, usare Connessione macchina virtuale (VMConnect) o [configurare una rete virtuale per la macchina virtuale](https://technet.microsoft.com/library/cc816585.aspx).  
  
## <a name="create-and-exit-a-powershell-direct-session-using-pssession-cmdlets"></a>Creare e chiudere una sessione di PowerShell Direct con i cmdlet PSSession  
  
1. Nell'host di Hyper-V aprire Windows PowerShell come amministratore.  
  
2. Usare il cmdlet [Enter-PSSession](https://technet.microsoft.com/library/hh849707.aspx) per connettersi alla macchina virtuale. Eseguire uno dei comandi seguenti per creare una sessione usando il nome o il GUID della macchina virtuale:  
  
    ```  
    Enter-PSSession -VMName <VMName>  
    ```  
  
    ```  
    Enter-PSSession -VMGUID <VMGUID>  
    ```  
  
3. Digitare le credenziali per la macchina virtuale.   
4. Eseguire tutti i comandi necessari. Questi comandi vengono eseguiti nella macchina virtuale con cui è stata creata la sessione.  
  
5.  Al termine, usare [Exit-PSSession](https://technet.microsoft.com/library/hh849743.aspx) per chiudere la sessione.   
  
    ```  
    Exit-PSSession  
    ```  
  
## <a name="run-script-or-command-with-invoke-command-cmdlet"></a>Eseguire uno script o un comando con il cmdlet Invoke-Command  
È possibile usare il cmdlet [Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command) per eseguire un set predeterminato di comandi nella macchina virtuale. Di seguito è riportato un esempio di come è possibile utilizzare il cmdlet Invoke-Command, dove PSTest è il nome della macchina virtuale e lo script da eseguire (foo.ps1) si trova nella cartella script nell'unità C:  
  
```  
Invoke-Command -VMName PSTest  -FilePath C:\script\foo.ps1  
```  
  
Per eseguire un comando singolo, utilizzare il parametro **-ScriptBlock**:  
  
```  
Invoke-Command -VMName PSTest  -ScriptBlock { cmdlet }  
```  
  
## <a name="whats-required-to-use-powershell-direct"></a>Cosa è necessario per usare PowerShell Direct?  
Per creare una sessione di PowerShell Direct in una macchina virtuale  
  
-   La macchina virtuale deve essere eseguita localmente nell'host e avviata.  
  
-   È necessario accedere al computer host come amministratore di Hyper-V.  
  
-   È necessario specificare credenziali utente valide per la macchina virtuale.  
  
-   Il sistema operativo host deve eseguire almeno Windows 10 o Windows Server 2016.
  
-   La macchina virtuale deve eseguire almeno Windows 10 o Windows Server 2016.  
  
È possibile usare il cmdlet [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) per verificare che le credenziali usate abbiano il ruolo di amministratore di Hyper-V e per ottenere un elenco delle macchine virtuali in esecuzione in locale nell'host e avviate.  
  
## <a name="see-also"></a>Vedi anche  
[Enter-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enter-PSSession)  
[Exit-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Exit-PSSession)  
[Invoke-Command](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Invoke-Command)  
  


