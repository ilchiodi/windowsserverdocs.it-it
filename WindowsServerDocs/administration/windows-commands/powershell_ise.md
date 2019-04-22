---
title: PowerShell_ise
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 32c41b5b-a210-47d9-bd8c-91eb9830b4f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03c765b276a2e61247661e132dd49434b444530c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817282"
---
# <a name="powershellise"></a>PowerShell_ise



Windows PowerShell Integrated Scripting Environment (ISE) è un'applicazione host con interfaccia grafica che consente di leggere, scrivere, eseguire, eseguire il debug e testare script e moduli in un ambiente grafico assistita. Le principali funzionalità quali IntelliSense, Show-Command, frammenti di codice, completamento tramite tasto tab, colorazione della sintassi, visual debug e Guida sensibile al contesto di offrire un'esperienza di scripting avanzata.

Il **PowerShell_ISE.exe** strumento si avvia una sessione di Windows PowerShell ISE. Quando si usa **PowerShell_ISE.exe**, è possibile usare i parametri facoltativi per aprire i file in Windows PowerShell ISE o avviare una sessione di Windows PowerShell ISE con alcun profilo o con un apartment a thread multipli.

**PowerShell_ISE.exe** è stata introdotta in Windows PowerShell 2.0 e ampliato in modo significativo in Windows PowerShell 3.0.

## <a name="using-powershelliseexe"></a>Usando PowerShell_ISE.exe

È possibile usare **PowerShell_ISE.exe** iniziare e terminare una sessione di Windows PowerShell come indicato di seguito:
-   Per avviare una sessione di Windows PowerShell ISE, in una finestra del prompt dei comandi, in Windows PowerShell o nel menu Start, digitare:  
    ```
    PowerShell_Ise
    ```  
-   Per aprire uno script (con estensione ps1), il modulo di script (psm1), manifesto del modulo (con estensione psd1), file XML o qualsiasi altro file supportato in Windows PowerShell ISE, usare il comando nel formato seguente:  
    ```
    PowerShell_Ise <FilePath>
    ```  
    In Windows PowerShell 3.0, è possibile usare l'opzione facoltativa **File** parametro come indicato di seguito:  
    ```
    PowerShell_Ise -File <FilePath>
    ```  
-   Per avviare una sessione di Windows PowerShell ISE senza i profili di Windows PowerShell, usare il **NoProfile** parametro. (Il **NoProfile** parametro è stato introdotto in Windows PowerShell 3.0.)  
    ```
    PowerShell_Ise -NoProfile
    ```  
-   Per vedere le **PowerShell_ISE.exe** contribuire file in una finestra del prompt dei comandi, usare il comando nel formato seguente:  
    ```
    PowerShell_Ise -help, -?, /?
    ```  
Per un elenco completo delle **PowerShell_ISE.exe** parametri della riga di comando, vedere [about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512).

## <a name="start-windows-powershell-ise-in-other-ways"></a>Avviare Windows PowerShell ISE in altri modi

Per informazioni su altri modi per avviare Windows PowerShell ISE, vedere [avvio di Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=135259).

## <a name="remarks"></a>Note

Windows PowerShell viene eseguito nell'opzione di installazione Server Core di sistemi operativi Windows Server. Tuttavia, poiché Windows PowerShell ISE richiede un'interfaccia utente grafica, non viene eseguito nelle installazioni Server Core.

## <a name="additional-references"></a>Altri riferimenti

[about_PowerShell_Ise.exe](https://go.microsoft.com/fwlink/?LinkId=256512)
[about_PowerShell.exe](https://go.microsoft.com/fwlink/?LinkID=113439)
[Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=107116)
[Scripting con Windows PowerShell](https://technet.microsoft.com/scriptcenter/dd742419) vedere anche