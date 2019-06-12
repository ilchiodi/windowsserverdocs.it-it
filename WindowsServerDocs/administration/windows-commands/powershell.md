---
title: PowerShell
description: Informazioni su come aprire la console di PowerShell da un prompt dei comandi.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 1e2ccf6187e4480f94b30632b6f8f9f092052541
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811081"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell è una shell da riga di comando basata su attività e un linguaggio di scripting progettato specificamente per l'amministrazione del sistema. Basato su .NET Framework, Windows PowerShell consente ai professionisti IT e agli utenti esperti di controllare e automatizzare l'amministrazione del sistema operativo Windows e delle applicazioni eseguite al suo interno.

Il **PowerShell.exe** lo strumento da riga di comando avvia una sessione di Windows PowerShell in una finestra del prompt dei comandi. Quando si usa **PowerShell.exe**, è possibile usare i parametri facoltativi per personalizzare la sessione. Ad esempio, è possibile avviare una sessione che usa un criterio di esecuzione particolare o uno che escluda un profilo di Windows PowerShell. In caso contrario, la sessione è quello utilizzato per qualsiasi sessione che viene avviato nella console di Windows PowerShell.

## <a name="using-powershellexe"></a>Usando PowerShell.exe

È possibile usare la **PowerShell.exe** lo strumento da riga di comando per avviare una sessione di Windows PowerShell in una finestra del prompt dei comandi.

- Per avviare una sessione di Windows PowerShell in una finestra del prompt dei comandi, digitare `PowerShell`. Oggetto **PS** prefisso viene aggiunto al prompt dei comandi per indicare che sei in una sessione di Windows PowerShell.

- Per avviare una sessione con un criterio di esecuzione specifico, usare il **ExecutionPolicy** parametro.

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Per avviare una sessione di Windows PowerShell senza i profili di Windows PowerShell, usare il **NoProfile** parametro.

    ```
    PowerShell.exe -NoProfile
    ```
  
- Per avviare una sessione, usare il **ExecutionPolicy** parametro.

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```
  
- Per visualizzare il PowerShell.exe file della Guida, usare il comando nel formato seguente.  
    
    ```
    PowerShell.exe -help, -?, /?
    ```

- Per terminare una sessione di Windows PowerShell in una finestra del prompt dei comandi, digitare `exit`. Restituisce il prompt dei comandi tipico.

Per un elenco completo delle **PowerShell.exe** parametri della riga di comando, vedere [about_PowerShell.Exe](https://go.microsoft.com/fwlink/?LinkID=113439).

## <a name="other-ways-to-start-windows-powershell"></a>Altri modi per avviare Windows PowerShell

Per informazioni su altri modi per avviare Windows PowerShell, vedere [avvio di Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Note

Windows PowerShell viene eseguito nell'opzione di installazione Server Core di sistemi operativi Windows Server. Tuttavia, le funzionalità che richiedono un utente di grafica dell'interfaccia, come le [Windows PowerShell Integrated Scripting Environment (ISE)](https://technet.microsoft.com/library/hh849182)e il [Out-GridView](https://go.microsoft.com/fwlink/?LinkID=113364) e [Show-Command](https://go.microsoft.com/fwlink/?LinkID=217448)i cmdlet, non vengono eseguiti nelle installazioni Server Core.

## <a name="additional-references"></a>Altri riferimenti

[about_PowerShell.exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[Scripting con Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) vedere anche