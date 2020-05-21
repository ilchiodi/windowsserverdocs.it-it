---
title: PowerShell
description: Informazioni su come aprire la console di PowerShell da un prompt dei comandi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 694fc970-0b6c-4046-b1b5-7eb1a0d26609
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: b733a187017293e8a33ff307b485380ef8f9b472
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436566"
---
# <a name="powershell"></a>PowerShell

Windows PowerShell è una shell da riga di comando basata su attività e un linguaggio di scripting progettato appositamente per l'amministrazione del sistema. Basato su .NET Framework, Windows PowerShell consente ai professionisti IT e agli utenti esperti di controllare e automatizzare l'amministrazione del sistema operativo Windows e delle applicazioni eseguite al suo interno.

Lo strumento da riga di comando **PowerShell. exe** avvia una sessione di Windows PowerShell in una finestra del prompt dei comandi. Quando si usa **PowerShell. exe**, è possibile usare i parametri facoltativi per personalizzare la sessione. Ad esempio, è possibile avviare una sessione che usa uno specifico criterio di esecuzione o uno che esclude un profilo di Windows PowerShell. In caso contrario, la sessione è identica a qualsiasi sessione avviata nella console di Windows PowerShell.

## <a name="using-powershellexe"></a>Uso di PowerShell. exe

È possibile usare lo strumento da riga di comando **PowerShell. exe** per avviare una sessione di Windows PowerShell in una finestra del prompt dei comandi.

- Per avviare una sessione di Windows PowerShell in una finestra del prompt dei comandi, digitare `PowerShell` . Al prompt dei comandi viene aggiunto un prefisso **PS** per indicare che ci si trova in una sessione di Windows PowerShell.

- Per avviare una sessione con determinati criteri di esecuzione, usare il parametro **ExecutionPolicy** .

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Per avviare una sessione di Windows PowerShell senza i profili di Windows PowerShell, usare il parametro **noprofile** .

    ```
    PowerShell.exe -NoProfile
    ```

- Per avviare una sessione, usare il parametro **ExecutionPolicy** .

    ```
    PowerShell.exe -ExecutionPolicy Restricted
    ```

- Per visualizzare il file della Guida di PowerShell. exe, usare il seguente formato di comando.

    ```
    PowerShell.exe -help, -?, /?
    ```

- Per terminare una sessione di Windows PowerShell in una finestra del prompt dei comandi, digitare `exit` . Il prompt dei comandi tipico restituisce.

Per un elenco completo dei parametri della riga di comando di **PowerShell. exe** , vedere [about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439).

## <a name="other-ways-to-start-windows-powershell"></a>Altri modi per avviare Windows PowerShell

Per informazioni su altri modi per avviare Windows PowerShell, vedere [avvio di Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Osservazioni

Windows PowerShell viene eseguito con l'opzione di installazione dei componenti di base del server dei sistemi operativi Windows Server. Tuttavia, le funzionalità che richiedono un'interfaccia utente grafica, ad esempio [Windows PowerShell Integrated Scripting Environment (ISE)](https://technet.microsoft.com/library/hh849182)e i cmdlet [Out-GridView](https://go.microsoft.com/fwlink/?LinkID=113364) e [show-Command](https://go.microsoft.com/fwlink/?LinkID=217448) , non vengono eseguite nelle installazioni dei componenti di base del server.

## <a name="additional-references"></a>Riferimenti aggiuntivi

[about_PowerShell. exe](https://go.microsoft.com/fwlink/?LinkID=113439) 
 [about_PowerShell_Ise. exe](https://go.microsoft.com/fwlink/?LinkId=256512) 
 [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116) 
 Creazione [di script con Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) Vedere anche