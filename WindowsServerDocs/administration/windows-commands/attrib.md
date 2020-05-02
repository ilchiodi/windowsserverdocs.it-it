---
title: attrib
description: Argomento di riferimento per il comando attrib, che Visualizza, imposta o rimuove gli attributi assegnati a file o directory.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c525fe1dc5b78032f20358492a1bfde4df909add
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719201"
---
# <a name="attrib"></a>attrib

Visualizza, imposta o rimuove gli attributi assegnati a file o directory. Se usato senza parametri, **attrib** Visualizza gli attributi di tutti i file nella directory corrente.

## <a name="syntax"></a>Sintassi

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<drive>:][<path>][<filename>] [/s [/d] [/l]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `{+|-}r` | Imposta (**+**) o Cancella (**-**) l'attributo di file di sola lettura. |
| `{+\|-}a` | Imposta (**+**) o Cancella (**-**) l'attributo del file di archivio. Questo set di attributi contrassegna i file che sono stati modificati dall'ultima volta in cui è stato eseguito il backup. Si noti che il comando **xcopy** utilizza gli attributi di archivio. |
| `{+\|-}s` | Imposta (**+**) o Cancella (**-**) l'attributo del file di sistema. Se un file usa questo set di attributi, è necessario cancellare l'attributo prima di poter modificare gli altri attributi per il file. |
| `{+\|-}h` | Imposta (**+**) o Cancella (**-**) l'attributo di file nascosto. Se un file usa questo set di attributi, è necessario cancellare l'attributo prima di poter modificare gli altri attributi per il file. |
| `{+\|-}i` | Imposta (**+**) o Cancella (**-**) l'attributo di file indicizzato non contenuto. |
| `[<drive>:][<path>][<filename>]` | Specifica il percorso e il nome della directory, del file o del gruppo di file per cui si desidera visualizzare o modificare gli attributi.<p>È possibile utilizzare il **?** e **&#42;** caratteri jolly nel parametro *filename* per visualizzare o modificare gli attributi per un gruppo di file. |
| /s | Applica **attrib** ed eventuali opzioni della riga di comando ai file corrispondenti nella directory corrente e in tutte le relative sottodirectory. |
| /d | Applica **attrib** ed eventuali opzioni della riga di comando alle directory. |
| /l | Applica **attrib** ed eventuali opzioni della riga di comando al collegamento simbolico, anziché alla destinazione del collegamento simbolico. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per visualizzare gli attributi di un file denominato News86 che si trova nella directory corrente, digitare:

```
attrib news86
```

Per assegnare l'attributo di sola lettura al file denominato report. txt, digitare:

```
attrib +r report.txt
```

Per rimuovere l'attributo di sola lettura dai file nella directory pubblica e nelle relative sottodirectory su un disco nell'unità b:, digitare:

```
attrib -r b:\public\*.* /s
```

Per impostare l'attributo Archive per tutti i file nell'unità a:, quindi deselezionare l'attributo Archive per i file con estensione bak, digitare:

```
attrib +a a:*.* & attrib -a a:*.bak
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando xcopy](xcopy.md)
