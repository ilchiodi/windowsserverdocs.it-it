---
title: dove
description: Windows Commands argomento per Where, che Visualizza il percorso dei file che corrispondono al criterio di ricerca specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0b3486a5-896b-4d92-84b8-e463a0b76487
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b32424622e8a893023aad9365b6aec4a91764fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829334"
---
# <a name="where"></a>dove



Visualizza il percorso dei file corrispondenti al criterio di ricerca specificato.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...] 
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/r \<dir >|Indica una ricerca ricorsiva, a partire dalla directory specificata.|
|/q|Restituisce un codice di uscita (**0** di esito positivo, **1** errore) senza visualizzare l'elenco di file corrispondenti.|
|/f|Visualizza i risultati del **in** comando tra virgolette.|
|/t|Visualizza le dimensioni del file e la data dell'ultima modifica e l'ora di ogni file corrispondente.|
|[$\<ENV >:\|percorso \<>:]\<pattern > [...]|Specifica il criterio di ricerca per i file devono corrispondere. È necessario almeno un criterio e il modello può includere caratteri jolly ( **&#42;** e **?** ). Per impostazione predefinita, **in** esamina la directory corrente e i percorsi specificati nella variabile di ambiente PATH. È possibile specificare un percorso diverso per la ricerca utilizzando il formato $*ENV*:*modello* (dove *ENV* è una variabile di ambiente esistente che contiene uno o più percorsi) o utilizzando il formato *percorso*:*modello* (dove *percorso* è il percorso della directory in cui cercare). Questi formati facoltativi non devono essere utilizzati con il **/r** opzione della riga di comando.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Se non si specifica un'estensione nome file, le estensioni elencate nella variabile di ambiente PATHEXT vengono aggiunti al modello per impostazione predefinita.
-   **Dove** possibile eseguire ricerche ricorsive, visualizzare informazioni sui file, ad esempio data o la dimensione e accettare le variabili di ambiente al posto di percorsi su computer locali.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)