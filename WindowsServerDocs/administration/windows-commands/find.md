---
title: trovare
description: Argomento di riferimento per il comando trova, che consente di cercare una stringa di testo nei file, visualizzando la stringa di testo specificata nel file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0273405ce5e5b4958a347cd1eaddee0a38897f0c
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437006"
---
# <a name="find"></a>trovare

Cerca una stringa di testo in una o più file e visualizza le righe di testo che contengono la stringa specificata.

## <a name="syntax"></a>Sintassi

```
find [/v] [/c] [/n] [/i] [/off[line]] <string> [[<drive>:][<path>]<filename>[...]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /v | Visualizza tutte le righe che non contengono l'oggetto specificato `<string>` . |
| /C | Conta le righe che contengono l'oggetto specificato `<string>` e visualizza il totale. |
| /n | Precede ogni riga con numero di riga del file. |
| /i | Specifica che la ricerca non fa distinzione maiuscole/minuscole. |
| [/ [offline]] | Non ignora i file in cui è impostato l'attributo offline. |
| `<string>` | Obbligatorio. Specifica il gruppo di caratteri (racchiusa tra virgolette singole) che si desidera cercare. |
| `[<drive>:][<path>]<filename>` | Specifica il percorso e nome del file in cui cercare la stringa specificata. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Se non si usa **/i**, questo comando cerca esattamente quanto specificato per la *stringa*. Ad esempio, questo comando considera i caratteri `a` e in `A` modo diverso. Se si usa **/i**, tuttavia, la ricerca non fa distinzione tra maiuscole e minuscole e considera `a` e `A` come lo stesso carattere.

- Se la stringa che si desidera cercare contiene virgolette, è necessario utilizzare le virgolette doppie per ogni virgoletta contenuta all'interno della stringa (ad esempio, "" questa stringa contiene le virgolette "").

- Se si omette un nome di file, questo comando funge da filtro, accettando input dall'origine di input standard (in genere la tastiera, una pipe (|) o un file reindirizzato) e quindi Visualizza tutte le righe che contengono la *stringa*.

- È possibile digitare parametri e opzioni della riga di comando per il comando **trova** in qualsiasi ordine.

- Non è possibile usare caratteri jolly (**&#42;** e **?**) nei nomi file o nelle estensioni specificate durante l'uso di questo comando. Per cercare una stringa in un set di file specificato con caratteri jolly, è possibile usare questo comando all'interno di un comando **for** .

- Se si usa **/c** e **/v** nella stessa riga di comando, questo comando Visualizza un conteggio delle righe che non contengono la stringa specificata. Se si specifica **/c** e **/n** nella stessa riga di comando, **trovare** Ignora **/n**.

- Questo comando non riconosce i ritorni a capo. Quando si utilizza questo comando per cercare testo in un file che include i ritorni a capo, è necessario limitare la stringa di ricerca al testo che è possibile trovare tra i ritorni a capo, ovvero una stringa che non è probabile che venga interrotta da un ritorno a capo. Questo comando, ad esempio, non segnala una corrispondenza per il file di imposta di stringa se si verifica un ritorno a capo tra le parole Tax e file.

### <a name="examples"></a>Esempi

Per visualizzare tutte le righe da *Pencil.ad* che contengono la stringa di *affilatura della matita*, digitare:

```
find pencil sharpener pencil.ad
```

Per trovare il testo "gli scienziati hanno etichettato il loro documento solo per una discussione. Non è un report finale." nel file *report. doc* , digitare:

```
find ""The scientists labeled their paper for discussion only. It is not a final report."" report.doc
```

Per cercare un set di file, è possibile usare il comando **trova** all'interno del comando **for** . Per eseguire una ricerca nella directory corrente per i file con estensione bat e che contengono la richiesta di stringa, digitare:

```
for %f in (*.bat) do find PROMPT %f
```

Per cercare il disco rigido per trovare e visualizzare i nomi dei file nell'unità C che contiene la CPU della stringa, usare la pipe (|) per indirizzare l'output del comando **dir** al comando **trova** , come indicato di seguito:

```
dir c:\ /s /b | find CPU
```

Poiché le ricerche di **ricerca** fanno distinzione tra maiuscole e minuscole e **dir** produce un output in maiuscolo, è necessario digitare la CPU della stringa in lettere maiuscole oppure usare l'opzione della riga di comando **/i** con **Find**.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [per il comando](for.md)