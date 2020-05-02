---
title: dove
description: Argomento di riferimento per Where, che Visualizza il percorso dei file che corrispondono al criterio di ricerca specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0b3486a5-896b-4d92-84b8-e463a0b76487
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4cec462e0d3652a20abb6290cd20b1d9d88aab53
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725811"
---
# <a name="where"></a>dove



Visualizza il percorso dei file corrispondenti al criterio di ricerca specificato.



## <a name="syntax"></a>Sintassi

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...] 
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/r \<dir>|Indica una ricerca ricorsiva, a partire dalla directory specificata.|
|/q|Restituisce un codice di uscita (**0** di esito positivo, **1** errore) senza visualizzare l'elenco di file corrispondenti.|
|/f|Visualizza i risultati del **in** comando tra virgolette.|
|/t|Visualizza le dimensioni del file e la data dell'ultima modifica e l'ora di ogni file corrispondente.|
|[$\<ENV>:\|\<percorso>:] \<Pattern> [...]|Specifica il criterio di ricerca per i file devono corrispondere. È necessario almeno un criterio e il modello può includere caratteri jolly (**&#42;** e **?**). Per impostazione predefinita, **in** esamina la directory corrente e i percorsi specificati nella variabile di ambiente PATH. È possibile specificare un percorso diverso per la ricerca utilizzando il formato $*ENV*:*modello* (dove *ENV* è una variabile di ambiente esistente che contiene uno o più percorsi) o utilizzando il formato *percorso*:*modello* (dove *percorso* è il percorso della directory in cui cercare). Questi formati facoltativi non devono essere utilizzati con il **/r** opzione della riga di comando.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Se non si specifica un'estensione nome file, le estensioni elencate nella variabile di ambiente PATHEXT vengono aggiunti al modello per impostazione predefinita.
-   **Dove** possibile eseguire ricerche ricorsive, visualizzare informazioni sui file, ad esempio data o la dimensione e accettare le variabili di ambiente al posto di percorsi su computer locali.

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)