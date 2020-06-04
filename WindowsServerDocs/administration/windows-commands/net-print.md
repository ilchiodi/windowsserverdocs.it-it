---
title: stampa NET
description: Argomento di riferimento per il comando NET Print. Questo comando è stato deprecato e non è garantito che sia supportato nelle versioni future di Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d8744c3ef4540652b495aea0037e97f433238f2
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354311"
---
# <a name="net-print"></a>stampa NET

> [!IMPORTANT]
> Questo comando è stato deprecato. Tuttavia, è possibile eseguire molte delle stesse attività usando il [comando prnjobs](prnjobs.md), [Strumentazione gestione Windows (WMI)](https://docs.microsoft.com/windows/win32/wmisdk/wmi-start-page), [PrintManagement in PowerShell](https://docs.microsoft.com/powershell/module/printmanagement)o [le risorse di script per i professionisti IT](https://gallery.technet.microsoft.com/ScriptCenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=printing&f%5B0%5D.Text=Printing).

Visualizza le informazioni relative a una coda di stampanti specificata o a un processo di stampa specificato oppure controlla un processo di stampa specificato.

## <a name="syntax"></a>Sintassi

```
net print {\\<computername>\<sharename> | \\<computername> <jobnumber> [/hold | /release | /delete]} [help]
```

### <a name="parameters"></a>Parametri

| Parametri | Description |
| ---------- | ----------- |
| `\\<computername>\<sharename>` | Specifica (per nome) il computer e la coda di stampa per cui si desidera visualizzare le informazioni. |
| `\\<computername>` | Specifica (per nome) il computer che ospita il processo di stampa che si desidera controllare. Se non si specifica un computer, viene utilizzato il computer locale. Richiede il `<jobnumber>` parametro. |
| `<jobnumber>` | Specifica il numero del processo di stampa che si desidera controllare. Questo numero viene assegnato dal computer che ospita la coda di stampa in cui viene inviato il processo di stampa. Dopo che un computer ha assegnato un numero a un processo di stampa, tale numero non viene assegnato ad altri processi di stampa in una coda ospitata da tale computer. Obbligatorio quando si usa il `\\<computername>` parametro. |
| `[/hold | /release | /delete]` | Specifica l'azione da eseguire con il processo di stampa. Se si specifica un numero di processo, ma non si specifica alcuna azione, verranno visualizzate le informazioni sul processo di stampa.<ul><li>**/Hold** -ritarda il processo, consentendo ad altri processi di stampa di ignorarlo fino a quando non viene rilasciato.</li><li>**/Release** : rilascia un processo di stampa che è stato posticipato.</li><li>**/Delete** : rimuove un processo di stampa da una coda di stampa.</li></ul> |
| help | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- Il `net print\\<computername>` comando Visualizza le informazioni sui processi di stampa in una coda della stampante condivisa. Di seguito è riportato un esempio di report per tutti i processi di stampa in una coda per una stampante condivisa denominata *laser*:

    ```
    printers at \\PRODUCTION
    Name              Job #      Size      Status
    -----------------------------
    LASER Queue       3 jobs               *printer active*
    USER1          84        93844      printing
    USER2          85        12555      Waiting
    USER3          86        10222      Waiting
    ```

- Di seguito è riportato un esempio di report per un processo di stampa:

    ```
    Job #            35
    Status           Waiting
    Size             3096
    remark
    Submitting user  USER2
    Notify           USER2
    Job data type
    Job parameters
    additional info
    ```

### <a name="examples"></a>Esempio

Per elencare il contenuto della coda di stampa *Dotmatrix* nel computer di * \\ produzione* , digitare:

```
net print \\Production\Dotmatrix
```

Per visualizzare informazioni sul numero di processo *35* nel computer di * \\ produzione* , digitare:

```
net print \\Production 35
```

Per ritardare il numero di processo *263* nel computer di * \\ produzione* , digitare:

```
net print \\Production 263 /hold
```

Per rilasciare il numero di processo *263* nel computer di * \\ produzione* , digitare:

```
net print \\Production 263 /release
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [riferimento al comando stampa](print-command-reference.md)

- [comando prnjobs](prnjobs.md)

- [Strumentazione gestione Windows (WMI, Windows Management Instrumentation)](https://docs.microsoft.com/windows/win32/wmisdk/wmi-start-page)

- [PrintManagement in PowerShell](https://docs.microsoft.com/powershell/module/printmanagement)

- [Risorse di script per professionisti IT](https://gallery.technet.microsoft.com/ScriptCenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=printing&f%5B0%5D.Text=Printing)
