---
title: Stampa di rete
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad631788a59c24dcb92d180330de25a5be320154
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839054"
---
# <a name="net-print"></a>Stampa di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le informazioni relative a una coda di stampanti specificata o a un processo di stampa specificato oppure controlla un processo di stampa specificato.
per esempi relativi all'uso di questo comando, vedere la sezione [esempi](#BKMK_examples) di questo documento.
> [!NOTE]
> Questo comando è stato deprecato in Windows 7 e Windows Server 2008 R2. Tuttavia, è possibile eseguire molte delle stesse attività usando i cmdlet di prnjobs, Strumentazione gestione Windows (WMI) o Windows PowerShell. Per ulteriori informazioni, vedere [prnjobs](prnjobs.md), [Strumentazione gestione Windows](https://go.microsoft.com/fwlink/?LinkID=29991) (https://go.microsoft.com/fwlink/?LinkID=29991), [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=128426) (https://go.microsoft.com/fwlink/?LinkID=128426)e [TechNet Script Center Gallery](https://go.microsoft.com/fwlink/?LinkId=164635) (https://go.microsoft.com/fwlink/?LinkId=164635).
> ## <a name="syntax"></a>Sintassi
> ```
> Net print {\\<computerName>\<Sharename> | 
> \\<computerName> <JobNumber> [/hold | /release | /delete]} [help]
> ```
> ### <a name="parameters"></a>Parametri
> 
> |               Parametri               |                                                                                                                                                                                                                     Descrizione                                                                                                                                                                                                                      |
> |----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |    \\\\<computerName>\\<Sharename>     |                                                                                                                                                                            Specifica (per nome) il computer e la coda di stampa per cui si desidera visualizzare le informazioni.                                                                                                                                                                             |
> |           \\\\<computerName>           |                                                                                                                                 Specifica (per nome) il computer che ospita il processo di stampa che si desidera controllare. Se non si specifica un computer, viene utilizzato il computer locale. Richiede il parametro <JobNumber>.                                                                                                                                  |
> |              <JobNumber>               |                                             Specifica il numero del processo di stampa che si desidera controllare. Questo numero viene assegnato dal computer che ospita la coda di stampa in cui viene inviato il processo di stampa. Dopo che un computer ha assegnato un numero a un processo di stampa, tale numero non viene assegnato ad altri processi di stampa in una coda ospitata da tale computer. Obbligatorio quando si usa il parametro \\\\<computerName>.                                             |
> | [/Hold &#124; /Release &#124; /Delete] | Specifica l'azione da eseguire con il processo di stampa.<p>-Il parametro **/Hold** ritarda il processo, consentendo ad altri processi di stampa di ignorarlo fino a quando non viene rilasciato.<br />-Il parametro **/Release** rilascia un processo di stampa che è stato posticipato.<br />-Il parametro **/Delete** rimuove un processo di stampa da una coda di stampa.<p>Se si specifica un numero di processo, ma non si specifica alcuna azione, verranno visualizzate le informazioni sul processo di stampa. |
> |                  help                  |                                                                                                                                                                                                     Visualizza la guida per il comando **net print** .                                                                                                                                                                                                     |
> 
> ## <a name="remarks"></a>Note
> - **Net print** \\\\<computerName> Visualizza informazioni sui processi di stampa in una coda di stampa condivisa. Di seguito è riportato un esempio di report per tutti i processi di stampa in una coda per una stampante condivisa denominata LASER:
>   ```
>   printers at \\PRODUCTION
>   Name              Job #      Size      Status
>   -----------------------------
>   LASER Queue       3 jobs               *printer active*
>      USER1          84        93844      printing
>      USER2          85        12555      Waiting
>      USER3          86        10222      Waiting
>   ```
> - Di seguito è riportato un esempio di report per un processo di stampa:
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
>   ## <a name="examples"></a><a name=BKMK_examples></a>Esempi
>   Questo esempio illustra come elencare il contenuto della coda di stampa Dotmatrix nel computer \\\Production:
>   ```
>   Net print \\Production\Dotmatrix 
>   ```
>   Questo esempio Mostra come visualizzare informazioni sul numero di processo 35 nel computer \\\Production:
>   ```
>   Net print \\Production 35 
>   ```
>   Questo esempio illustra come ritardare il numero di processo 263 nel computer \\\Production:
>   ```
>   Net print \\Production 263 /hold 
>   ```
>   Questo esempio Mostra come rilasciare il numero di processo 263 nel computer \\\Production:
>   ```
>   Net print \\Production 263 /release 
>   ```
>   ## <a name="additional-references"></a>Altre informazioni di riferimento
>   - [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
>   [riferimento al comando stampa](print-command-reference.md)
