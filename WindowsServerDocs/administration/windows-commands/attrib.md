---
title: attrib
description: Windows Commands argomento per **attrib**, che Visualizza, imposta o rimuove gli attributi assegnati a file o directory.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94a6f307450b06dff81180b70f864bd9e1ed5885
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851254"
---
# <a name="attrib"></a>attrib

Visualizza, imposta o rimuove gli attributi assegnati a file o directory. Se usato senza parametri, **attrib** Visualizza gli attributi di tutti i file nella directory corrente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<Drive>:][<Path>][<FileName>] [/s [/d] [/l]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `{+|-}r` | Imposta ( **+** ) o cancella ( **-** ) l'attributo di file di sola lettura. |
| `{+\|-}a` | Imposta ( **+** ) o cancella ( **-** ) l'attributo del file di archivio. |
| `{+\|-}s` | Imposta ( **+** ) o cancella ( **-** ) l'attributo del file di sistema. |
| `{+\|-}h` | Imposta ( **+** ) o cancella ( **-** ) l'attributo di file nascosto. |
| `{+\|-}i` | Imposta ( **+** ) o cancella ( **-** ) l'attributo di file indicizzato non contenuto. |
| `[<Drive>:][<Path>][<FileName>]` | Specifica il percorso e il nome della directory, del file o del gruppo di file per cui si desidera visualizzare o modificare gli attributi. È possibile utilizzare il **?** e **&#42;** caratteri jolly nel parametro *filename* per visualizzare o modificare gli attributi per un gruppo di file. |
| /s | Applica **attrib** ed eventuali opzioni della riga di comando ai file corrispondenti nella directory corrente e in tutte le relative sottodirectory. |
| /d | Applica **attrib** ed eventuali opzioni della riga di comando alle directory. |
| /l | Applica **attrib** ed eventuali opzioni della riga di comando al collegamento simbolico, anziché alla destinazione del collegamento simbolico. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="remarks"></a>Note

- È possibile utilizzare caratteri jolly ( **?** e **&#42;** ) con il parametro *filename* per visualizzare o modificare gli attributi per un gruppo di file.

- Se per un file è impostato l'attributo System (**s**) o Hidden (**h**), è necessario cancellare l'attributo prima di poter modificare gli altri attributi per il file.

- L'attributo Archive (**a**) contrassegna i file che sono stati modificati dall'ultima volta in cui è stato eseguito il backup. Si noti che il comando **xcopy** utilizza gli attributi di archivio.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare gli attributi di un file denominato News86 che si trova nella directory corrente, digitare:

```
attrib news86 
```

Per assegnare l'attributo di sola lettura al file denominato report. txt, digitare:

```
attrib +r report.txt 
```

Per rimuovere l'attributo di sola lettura dai file della directory pubblica e delle relative sottodirectory su un disco nell'unità B, digitare:

```
attrib -r b:\public\*.* /s 
```

Per impostare l'attributo Archive per tutti i file nell'unità A, quindi deselezionare l'attributo Archive per i file con estensione bak, digitare:

```
attrib +a a:*.* & attrib -a a:*.bak 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)