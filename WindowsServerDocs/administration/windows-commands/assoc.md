---
title: assoc
description: Argomento di riferimento per il comando Assoc, che consente di visualizzare o modificare le associazioni di estensione del nome file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 58735201a1a0711db4d0cee9c292363acf5121f3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819641"
---
# <a name="assoc"></a>assoc

Visualizza o modifica le associazioni di estensione del nome file. Se usato senza parametri, **Assoc** Visualizza un elenco di tutte le associazioni di estensione del nome di file corrente.

> [!NOTE]
> Questo comando è supportato solo in cmd. exe e non è disponibile da PowerShell.

## <a name="syntax"></a>Sintassi

```
assoc [<.ext>[=[<filetype>]]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<.ext>` | Specifica l'estensione del nome file. |
| `<filetype>` | Specifica il tipo di file da associare all'estensione del nome file specificata. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="remarks"></a>Osservazioni

- Per rimuovere l'associazione del tipo di file per un'estensione di file, aggiungere uno spazio vuoto dopo il segno di uguale premendo la barra SPAZIAtrice.

- Per visualizzare i tipi di file correnti con stringhe di comando aperte definite, utilizzare il comando **ftype** .

- Per reindirizzare l'output di **Assoc** a un file di testo, usare l' `>` operatore di reindirizzamento.

## <a name="examples"></a>Esempi

Per visualizzare l'associazione del tipo di file corrente per l'estensione txt del nome file, digitare:

```
assoc .txt
```

Per rimuovere l'associazione del tipo di file per l'estensione bak del nome file, digitare:

```
assoc .bak=
```

> [!NOTE]
> Assicurarsi di aggiungere uno spazio dopo il segno di uguale.

Per visualizzare l'output di **Assoc** una schermata alla volta, digitare:

```
assoc | more
```

Per inviare l'output di **Assoc** al file Assoc. txt, digitare:

```
assoc>assoc.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando ftype](ftype.md)
