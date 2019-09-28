---
title: dove
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0b3486a5-896b-4d92-84b8-e463a0b76487
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abebe5799075653d2ace1af4eadbdd5d477d97a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362165"
---
# <a name="where"></a>dove



Visualizza il percorso dei file corrispondenti al criterio di ricerca specificato.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...] 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/r \<Dir >|Indica una ricerca ricorsiva, a partire dalla directory specificata.|
|/q|Restituisce un codice di uscita (**0** di esito positivo, **1** errore) senza visualizzare l'elenco di file corrispondenti.|
|/f|Visualizza i risultati del **in** comando tra virgolette.|
|/t|Visualizza le dimensioni del file e la data dell'ultima modifica e l'ora di ogni file corrispondente.|
|[$ \<ENV >: \| @ no__t-2Path >:] \<Pattern > [...]|Specifica il criterio di ricerca per i file devono corrispondere. È necessario almeno un criterio e il modello può includere caratteri jolly ( **&#42;** e **?** ). Per impostazione predefinita, **in** esamina la directory corrente e i percorsi specificati nella variabile di ambiente PATH. È possibile specificare un percorso diverso per la ricerca utilizzando il formato $*ENV*:*modello* (dove *ENV* è una variabile di ambiente esistente che contiene uno o più percorsi) o utilizzando il formato *percorso*:*modello* (dove *percorso* è il percorso della directory in cui cercare). Questi formati facoltativi non devono essere utilizzati con il **/r** opzione della riga di comando.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Se non si specifica un'estensione nome file, le estensioni elencate nella variabile di ambiente PATHEXT vengono aggiunti al modello per impostazione predefinita.
-   **Dove** possibile eseguire ricerche ricorsive, visualizzare informazioni sui file, ad esempio data o la dimensione e accettare le variabili di ambiente al posto di percorsi su computer locali.

## <a name="BKMK_examples"></a>Esempi

Per trovare tutti i file denominati Test nell'unità C del computer in uso e nelle relative sottodirectory, digitare:
```
where /r c:\ test 
```
Per elencare tutti i file nella directory pubblica, digitare:
```
where $public:*.*
```
Per trovare tutti i file denominati blocco note nell'unità C del computer remoto, Computer1 e nelle relative sottodirectory, digitare:
```
where /r \\computer1\c notepad.*
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)