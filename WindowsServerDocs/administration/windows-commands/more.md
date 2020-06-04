---
title: more
description: Argomento di riferimento per il comando altro, che visualizza una schermata di output alla volta.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/26/2019
ms.openlocfilehash: 042669aa638990375157d08d9e12840ade486165
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354571"
---
# <a name="more"></a>more

Visualizza una schermata di output alla volta.

> [!NOTE]
> Il comando **altro** , con parametri diversi, è disponibile anche dalla console di ripristino.

## <a name="syntax"></a>Sintassi

```
<command> | more [/c] [/p] [/s] [/t<n>] [+<n>]
more [[/c] [/p] [/s] [/t<n>] [+<n>]] < [<drive>:][<path>]<filename>
more [/c] [/p] [/s] [/t<n>] [+<n>] [<files>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<command>` | Specifica un comando per il quale si desidera visualizzare l'output. |
| /C | Cancella lo schermo prima di visualizzare una pagina. |
| / p | Espande i caratteri del feed di form. |
| /s | Visualizza più righe vuote come una singola riga vuota. |
| /t`<n>` | Visualizza le schede come numero di spazi specificato da *n*. |
| +`<n>` | Visualizza il primo file, a partire dalla riga specificata da *n*. |
| `[<drive>:][<path>]<filename>` | Specifica il percorso e il nome di un file da visualizzare. |
| `<files>` | Specifica un elenco di file da visualizzare. I file devono essere separati tramite spazi. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Commenti

- I sottocomandi seguenti sono accettati al prompt **più** ( `-- More --` ), tra cui:

    | Chiave | Azione |
    | --- | ------ |
    | BARRA SPAZIATRICE | Premere la **barra spaziatrice** per visualizzare la schermata successiva. |
    | INVIO | Premere **invio** per visualizzare il file una riga alla volta. |
    | f | Premere **F** per visualizzare il file successivo elencato nella riga di comando. |
    | q | Premere **Q** per uscire dal comando **altro** . |
    | = | Mostra il numero di riga. |
    | p`<n>` | Premere **P** per visualizzare le *n* righe successive. |
    | s`<n>` | Premere **S** per ignorare le *n* righe successive. |
    | ? | Premere **?** per visualizzare i comandi disponibili al prompt **più** .|

- Se si utilizza il carattere di reindirizzamento ( `<` ), è necessario specificare anche un nome file come origine.

- Se si utilizza pipe ( `|` ), è possibile utilizzare tali comandi come **dir**, **Sort**e **Type**.

### <a name="examples"></a>Esempio

Per visualizzare la prima schermata di informazioni di un file denominato *clients. New*, digitare uno dei comandi seguenti:

```
more < clients.new
type clients.new | more
```

Il comando **altro** Visualizza la prima schermata di informazioni da clients. New ed è possibile premere la barra spaziatrice per visualizzare la schermata successiva delle informazioni.

Per cancellare la schermata e rimuovere tutte le righe vuote aggiuntive prima di visualizzare il file *clients. New*, digitare uno dei comandi seguenti:

```
more /c /s < clients.new
type clients.new | more /c /s
```

Per visualizzare il numero di riga corrente al prompt **più** , digitare:

```
more =
```

Il numero di riga corrente viene aggiunto al prompt **più** , come`-- More [Line: 24] --`

Per visualizzare un numero di righe specifico al prompt **più** , digitare:

```
more p
```

Il messaggio **più** rapido richiede il numero di righe da visualizzare, come segue: `-- More -- Lines:` . Digitare il numero di righe da visualizzare e quindi premere INVIO. La schermata viene modificata in modo da visualizzare solo tale numero di righe.

Per ignorare un numero specifico di righe **al prompt dei** comandi, digitare:

```
more s
```

Il messaggio **più** rapido richiede il numero di righe da ignorare, come segue: `-- More -- Lines:` . Digitare il numero di righe da ignorare, quindi premere INVIO. La schermata cambia per indicare che tali righe vengono ignorate.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Ambiente ripristino Windows (WinRE)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
