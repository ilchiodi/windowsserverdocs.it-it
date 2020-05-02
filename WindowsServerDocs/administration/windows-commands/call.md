---
title: chiamare
description: Argomento di riferimento per il comando call, che chiama un programma batch da un altro senza arrestare il programma batch padre.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d34a41dc-e6c7-4467-bf6a-15cec704833e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 64c4b89d18ab869a7e6c8b1ee8537c4f808bce8f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719663"
---
# <a name="call"></a>chiamare

Chiama un programma batch da un altro senza arrestare il programma batch padre. Il comando **Call** accetta le etichette come destinazione della chiamata

> [!NOTE]
> La chiamata non ha effetto al prompt dei comandi quando viene usata all'esterno di uno script o di un file batch.

## <a name="syntax"></a>Sintassi

```
call [drive:][path]<filename> [<batchparameters>] [:<label> [<arguments>]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `[<drive>:][<path>]<filename>` | Specifica il percorso e il nome del programma batch che si desidera chiamare. Il `<filename>` parametro è obbligatorio e deve avere un'estensione. bat o. cmd. |
| `<batchparameters>` | Specifica le informazioni della riga di comando richieste dal programma batch. |
| `:<label>` | Specifica l'etichetta a cui si desidera passare un controllo del programma batch. |
| `<arguments>` | Specifica le informazioni della riga di comando da passare alla nuova istanza del programma batch, a partire da `:<label>`.|
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="batch-parameters"></a>Parametri batch

I riferimenti all'argomento dello script batch (**%0**, **%1**,...) sono elencati nelle tabelle seguenti.

L'utilizzo del valore **% &#42;** in uno script batch fa riferimento a tutti gli argomenti (ad esempio, **%1**, **%2**, **%3**...).

È possibile utilizzare le seguenti sintassi facoltative come sostituzioni per i parametri batch (**% n**):

| Parametro batch | Descrizione |
| --------------- | ----------- |
| % ~ 1 | Espande **%1** e rimuove le virgolette circostanti. |
| % ~ F1 | Espande **%1** in un percorso completo. |
| % ~ D1 | Espande **%1** solo a una lettera di unità. |
| % ~ P1 | Espande **%1** solo in un percorso. |
| % ~ N1 | Espande **%1** solo in un nome file. |
| % ~ X1 | Espande **%1** solo in un'estensione del nome di file. |
| % ~ S1 | Espande **%1** in un percorso completo che contiene solo nomi brevi. |
| % ~ a1 | Espande **%1** sugli attributi del file. |
| % ~ T1 | Espande **%1** alla data e all'ora del file. |
| % ~ Z1 | Espande **%1** fino alla dimensione del file. |
| % ~ $PATH: 1 | Esegue la ricerca nelle directory elencate nella variabile di ambiente PATH ed espande **%1** con il nome completo della prima directory trovata. Se il nome della variabile di ambiente non è definito o il file non viene trovato dalla ricerca, questo modificatore espande la stringa vuota. |

La tabella seguente illustra come è possibile combinare i modificatori con i parametri batch per i risultati composti:

| Parametro batch con modificatore | Descrizione |
| ----------------------------- | ----------- |
| % ~ DP1 | Espande **%1** in una lettera di unità e un solo percorso. |
| % ~ nx1 | Espande **%1** solo in un nome file e un'estensione. |
| % ~ dp $ PATH: 1 | Esegue la ricerca nelle directory elencate nella variabile di ambiente PATH per **%1**, quindi si espande alla lettera di unità e al percorso della prima directory trovata. |
| % ~ ftza1 | Espande **%1** per visualizzare un output simile al comando **dir** . |

Negli esempi precedenti, **%1** e Path possono essere sostituiti da altri valori validi. La **%~** sintassi viene terminata con un numero di argomento valido. Impossibile **%~** utilizzare i modificatori con **% &#42;**.

### <a name="remarks"></a>Osservazioni

- Uso dei parametri batch:

    I parametri batch possono contenere tutte le informazioni che è possibile passare a un programma batch, incluse le opzioni della riga di comando, i nomi file, i parametri batch da **%0** a **%9**e le variabili (ad esempio, **% baud%**).

- Utilizzando il `<label>` parametro:

    Utilizzando **Call** con il `<label>` parametro, si crea un nuovo contesto del file batch e si passa il controllo all'istruzione dopo l'etichetta specificata. La prima volta che viene rilevata la fine del file batch (ovvero dopo il passaggio all'etichetta), il controllo torna all'istruzione successiva all'istruzione **Call** . La seconda volta che viene rilevata la fine del file batch, lo script batch viene terminato.

- Uso di pipe e simboli di reindirizzamento:

    Non `(|)` usare pipe o simboli di Reindirizzamento`<` (o `>`) con **Call**.

- Esecuzione di una chiamata ricorsiva

    È possibile creare un programma batch che chiama se stesso. Tuttavia, è necessario fornire una condizione di uscita. In caso contrario, i programmi batch padre e figlio possono eseguire un ciclo infinito.

- Utilizzo di estensioni di comando

    Se sono abilitate le estensioni **call** dei comandi `<label>` , la chiamata accetta come destinazione della chiamata. La sintassi corretta è`call :<label> <arguments>`

## <a name="examples"></a>Esempi

Per eseguire il programma denominato nuovo. bat da un altro programma batch, digitare il comando seguente nel programma batch padre:

```
call checknew
```

Se il programma batch padre accetta due parametri batch e si desidera che i parametri vengano passati a denominato nuovo. bat, digitare il comando seguente nel programma batch padre:

```
call checknew %1 %2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
