---
title: modifica
description: Argomento di riferimento per il comando modifica, che avvia l'editor MS-DOS, in modo che sia possibile creare e modificare i file di testo ASCII.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9f6c78889f466015d60149c27a87dcefe840133
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436916"
---
# <a name="edit"></a>modifica

Avvia l'editor di MS-DOS, che consente di creare e modificare i file di testo ASCII.

## <a name="syntax"></a>Sintassi

```
edit [/b] [/h] [/r] [/s] [/<nnn>] [[<drive>:][<path>]<filename> [<filename2> [...]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `[<drive>:][<path>]<filename> [<filename2> [...]]` | Specifica il percorso e il nome di uno o più file di testo ASCII. Se il file non esiste, l'editor di MS-DOS lo crea. Se il file esiste, MS-DOS Editor viene aperto e visualizzato il contenuto dello schermo. L'opzione *filename* può contenere caratteri jolly (**&#42;** e **?**). Separare più nomi di file con uno spazio. |
| / b | Modalità monocromatica forza, in modo che Editor MS-DOS vengono visualizzati in bianco e nero. |
| /h | Visualizza il numero massimo di righe consentito per il monitoraggio corrente. |
| /r | Carica i file in modalità sola lettura. |
| /s | Impone l'utilizzo di nomi di file brevi. |
| `<nnn>` | Carica i file binari, eseguendo il wrapping delle righe su *nnn* caratteri. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Per ulteriori informazioni, aprire l'Editor di MS-DOS e quindi premere il tasto F1.

- Per impostazione predefinita, alcuni monitoraggi non supportano la visualizzazione dei tasti di scelta rapida. Se il monitoraggio non Visualizza i tasti di scelta rapida, usare **/b**.

### <a name="examples"></a>Esempi

Per aprire l'Editor di MS-DOS, digitare:

```
edit
```

Per creare e modificare un file denominato *newtextfile. txt* nella directory corrente, digitare:

```
edit newtextfile.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
