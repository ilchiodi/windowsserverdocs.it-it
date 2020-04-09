---
title: assoc
description: Windows Commands Topic for Assoc, che consente di visualizzare o modificare le associazioni dell'estensione di file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 442ba244a7325425df29a1ebdcdb8bc107095ebe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851294"
---
# <a name="assoc"></a>assoc

Visualizza o modifica le associazioni di estensione del nome file. Se usato senza parametri, **Assoc** Visualizza un elenco di tutte le associazioni di estensione del nome di file corrente.

> [!NOTE]
> Questo comando è supportato solo all'interno di CMD. EXE e non è disponibile da PowerShell.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
assoc [<.ext>[=[<FileType>]]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<.ext>` | Specifica l'estensione del nome file. |
| `<FileType>` | Specifica il tipo di file da associare all'estensione del nome file specificata. |
| `/?` | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Note

- Per rimuovere l'associazione del tipo di file per un'estensione di file, aggiungere uno spazio vuoto dopo il segno di uguale premendo la barra SPAZIAtrice.

- Per visualizzare i tipi di file correnti con stringhe di comando aperte definite, utilizzare il comando **ftype** .

- Per reindirizzare l'output di **Assoc** a un file di testo, usare l'operatore di Reindirizzamento **>** .

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
