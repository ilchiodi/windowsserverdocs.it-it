---
title: PowerShell_ise
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8529e19e30b72c9b9c8f8c30e1ca39c5e8f1f40e
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436526"
---
# <a name="powershell_ise"></a>PowerShell_ise



Windows PowerShell Integrated Scripting Environment (ISE) è un'applicazione host grafica che consente di leggere, scrivere, eseguire, eseguire il debug e testare script e moduli in un ambiente basato su grafica. Funzionalità chiave quali IntelliSense, show-Command, frammenti, completamento tramite TAB, colorazione della sintassi, debug visivo e Guida sensibile al contesto forniscono un'esperienza di scripting avanzata.

Lo strumento **PowerShell_ISE. exe** avvia una sessione di Windows PowerShell ISE. Quando si usa **PowerShell_ISE. exe**, è possibile usare i parametri facoltativi per aprire i file in Windows PowerShell ISE o per avviare una sessione di Windows PowerShell ISE senza profilo o con un Apartment a thread multipli.

**PowerShell_ISE. exe** è stato introdotto in windows PowerShell 2,0 ed è stato ampliato in modo significativo in windows PowerShell 3,0.

## <a name="using-powershell_iseexe"></a>Utilizzo di PowerShell_ISE. exe

È possibile utilizzare **PowerShell_ISE. exe** per avviare e terminare una sessione di Windows PowerShell nel modo seguente:
- Per avviare una sessione di Windows PowerShell ISE, in una finestra del prompt dei comandi, in Windows PowerShell o nel menu Start, digitare:
  ```
  PowerShell_Ise
  ```
- Per aprire uno script (con estensione ps1), un modulo di script (. psm1), un manifesto del modulo (. psd1), un file XML o qualsiasi altro file supportato in Windows PowerShell ISE, usare il formato di comando seguente:
  ```
  PowerShell_Ise <FilePath>
  ```
  In Windows PowerShell 3,0 è possibile usare il parametro facoltativo **file** come indicato di seguito:
  ```
  PowerShell_Ise -File <FilePath>
  ```
- Per avviare una sessione di Windows PowerShell ISE senza i profili di Windows PowerShell, usare il parametro **noprofile** . Il parametro **noprofile** è stato introdotto in Windows PowerShell 3,0.
  ```
  PowerShell_Ise -NoProfile
  ```
- Per visualizzare il file della Guida **PowerShell_ISE. exe** in una finestra del prompt dei comandi, usare il seguente formato di comando:
  ```
  PowerShell_Ise -help, -?, /?
  ```
  Per un elenco completo dei parametri della riga di comando **PowerShell_ISE. exe** , vedere [about_PowerShell_Ise. exe](https://go.microsoft.com/fwlink/?LinkId=256512).

## <a name="start-windows-powershell-ise-in-other-ways"></a>Avviare Windows PowerShell ISE in altri modi

Per informazioni su altri modi per iniziare Windows PowerShell ISE, vedere [avvio di Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Osservazioni

Windows PowerShell viene eseguito con l'opzione di installazione dei componenti di base del server dei sistemi operativi Windows Server. Tuttavia, poiché Windows PowerShell ISE richiede un'interfaccia utente grafica, non viene eseguita nelle installazioni Server Core.

## <a name="additional-references"></a>Riferimenti aggiuntivi

[about_PowerShell_Ise. exe](https://go.microsoft.com/fwlink/?LinkId=256512) 
 [about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439) 
 [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116) 
 Creazione [di script con Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) Vedere anche