---
title: Stampa di rete
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4018ec3779a9735916136fa54f532ad5767c960e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437104"
---
# <a name="net-print"></a>Stampa di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni su una coda di stampa specificato o un processo di stampa specificato, o controlla un processo di stampa specificato.
Per esempi di come usare questo comando, vedere la [esempi](#BKMK_examples) sezione di questo documento.
> [!NOTE]
> Questo comando è stato deprecato in Windows 7 e Windows Server 2008 R2. Tuttavia, è possibile eseguire molte delle attività usando prnjobs, Strumentazione gestione Windows (WMI) o i cmdlet di Windows PowerShell. Per altre informazioni, vedere [prnjobs](prnjobs.md), [Strumentazione gestione Windows](https://go.microsoft.com/fwlink/?LinkID=29991) (https://go.microsoft.com/fwlink/?LinkID=29991), [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=128426) (https://go.microsoft.com/fwlink/?LinkID=128426)e il [Raccolta di TechNet Script Center](https://go.microsoft.com/fwlink/?LinkId=164635) (https://go.microsoft.com/fwlink/?LinkId=164635).
> ## <a name="syntax"></a>Sintassi
> ```
> Net print {\\<computerName>\<Sharename> | 
> \\<computerName> <JobNumber> [/hold | /release | /delete]} [help]
> ```
> ## <a name="parameters"></a>Parametri
> 
> |               Parametri               |                                                                                                                                                                                                                     Descrizione                                                                                                                                                                                                                      |
> |----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |    \\\\<computerName>\\<Sharename>     |                                                                                                                                                                            Specifica (nome) la coda di stampa e i computer su cui si desidera visualizzare le informazioni.                                                                                                                                                                             |
> |           \\\\<computerName>           |                                                                                                                                 Specifica (nome) del computer che ospita il processo di stampa che si desidera controllare. Se non si specifica un computer, verrà utilizzato il computer locale. Richiede il <JobNumber> parametro.                                                                                                                                  |
> |              <JobNumber>               |                                             Specifica il numero del processo di stampa che si desidera controllare. Questo numero viene assegnato dal computer che ospita la coda di stampa in cui viene inviato il processo di stampa. Dopo che un computer assegnato un numero per un processo di stampa, che numero non è assegnato a eventuali altri processi di stampa in una coda ospitata da tale computer. Obbligatorio quando si usa la \\ \\ <computerName> parametro.                                             |
> | [/hold &#124; /release &#124; /delete] | Specifica l'azione da intraprendere con il processo di stampa.<br /><br />-il **/contenere** parametro consente di ritardare il processo, consentendo di altri processi di stampa evitare l'operazione fino a quando non viene rilasciato.<br />-il **/rilascio** parametro rilascia un processo di stampa che è stato posticipato.<br />-il **/Elimina** parametro rimuove un processo di stampa da una coda di stampa.<br /><br />Se si specifica un numero di processi, ma non si specifica alcuna azione, informazioni sul processo di stampa viene visualizzate. |
> |                  help                  |                                                                                                                                                                                                     Visualizza la Guida per la **Net stampa** comando.                                                                                                                                                                                                     |
> 
> ## <a name="remarks"></a>Note
> - **NET print** \\ \\ <computerName> Visualizza informazioni sui processi di stampa in una coda di stampa condivisa. Di seguito è riportato un esempio di un report per tutti i processi di stampa in una coda per una stampante condivisa denominata LASER:
>   ```
>   printers at \\PRODUCTION
>   Name              Job #      Size      Status
>   -----------------------------
>   LASER Queue       3 jobs               *printer active*
>      USER1          84        93844      printing
>      USER2          85        12555      Waiting
>      USER3          86        10222      Waiting
>   ```
> - Di seguito è riportato un esempio di un report per un processo di stampa:
>   ```
>   Job #            35
>   Status           Waiting
>   Size             3096
>   remark
>   Submitting user  USER2
>   Notify           USER2
>   Job data type
>   Job parameters
>   additional info
>   ```
>   ## <a name="BKMK_examples"></a>Esempi
>   Questo esempio illustra come elencare il contenuto della coda di stampa Dotmatrix sul \\\Production computer:
>   ```
>   Net print \\Production\Dotmatrix 
>   ```
>   In questo esempio viene illustrato come visualizzare informazioni sul numero di processi 35 nel \\\Production computer:
>   ```
>   Net print \\Production 35 
>   ```
>   Questo esempio illustra come rimandare il numero di processi 263 sul \\\Production computer:
>   ```
>   Net print \\Production 263 /hold 
>   ```
>   Questo esempio illustra come rilasciare il numero di processi 263 sul \\\Production computer:
>   ```
>   Net print \\Production 263 /release 
>   ```
>   #### <a name="additional-references"></a>Riferimenti aggiuntivi
>   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
>   [riferimenti ai comandi di stampa](print-command-reference.md)
