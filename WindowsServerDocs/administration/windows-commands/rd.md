---
title: rd
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 298e6b291a6aa08701b6d54a11470b0cc4bea486
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836714"
---
# <a name="rd"></a>rd



Elimina una directory. Questo comando corrisponde al comando **rmdir** .

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

### <a name="parameters"></a>Parametri

|     Parametro     |                                                                 Descrizione                                                                  |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [\<unità >:]<Path> |                      Specifica il percorso e il nome della directory che si desidera eliminare. Il *percorso* è obbligatorio.                       |
|        /s         |                     Elimina un albero di directory (la directory specificata e tutte le relative sottodirectory, inclusi tutti i file).                      |
|        /q         | Specifica la modalità non interattiva. Non richiede la conferma dell'eliminazione di un albero di directory. Si noti che **/q** funziona solo se è specificato **/s** . |
|        /?         |                                                     Visualizza la guida al prompt dei comandi.                                                     |

## <a name="remarks"></a>Note

-   Non è possibile eliminare una directory che contiene file, inclusi i file di sistema o nascosti. Se si tenta di eseguire questa operazione, viene visualizzato il messaggio seguente:

    `The directory is not empty`

    Usare il comando **dir/a** per elencare tutti i file (inclusi i file di sistema e nascosti). Usare quindi il comando **attrib** con **-h** per rimuovere gli attributi di file nascosti, **-s** per rimuovere gli attributi del file di sistema o **-h-s** per rimuovere gli attributi di file di sistema e nascosti. Dopo la rimozione degli attributi nascosti e file, è possibile eliminare i file.
-   Se si inserisce una barra rovesciata (\) all'inizio del *percorso*, il *percorso* inizierà dalla directory radice (indipendentemente dalla directory corrente).
-   Non è possibile utilizzare **Rd** per eliminare la directory corrente. Se si tenta di eliminare la directory corrente, viene visualizzato il messaggio di errore seguente:

    `The process cannot access the file because it is being used by another process.`

    Se viene visualizzato questo messaggio di errore, è necessario passare a una directory diversa, non a una sottodirectory della directory corrente, quindi utilizzare **Rd** (specificare il *percorso* , se necessario).
-   Il comando **Rd** , con parametri diversi, è disponibile dalla console di ripristino.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Non è possibile eliminare la directory attualmente in uso. È necessario passare a una directory che non si trova nella directory corrente. Ad esempio, per passare alla directory padre, digitare:
```
cd ..
```
È ora possibile rimuovere in modo sicuro la directory desiderata.

Usare l'opzione **/s** per rimuovere un albero di directory. Per rimuovere, ad esempio, una directory denominata test (e tutte le relative sottodirectory e file) dalla directory corrente, digitare:
```
rd /s test
```
Per eseguire l'esempio precedente in modalità non interattiva, digitare:
```
rd /s /q test
```

> [!CAUTION]
> Quando si esegue **Rd/s** in modalità non interattiva, l'intero albero di directory viene eliminato senza conferma. Assicurarsi che i file importanti vengano spostati o sottoposti a backup prima di usare l'opzione della riga di comando **/q** .

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)