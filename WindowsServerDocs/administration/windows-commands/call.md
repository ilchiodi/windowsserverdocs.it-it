---
title: chiamare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d34a41dc-e6c7-4467-bf6a-15cec704833e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 1f5253700f2932b2afa725163121e64ea4c1748d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434586"
---
# <a name="call"></a>chiamare



Richiama un programma batch da un'altra senza interrompere il programma batch. Il **chiamare** comando accetta le etichette come destinazione della chiamata.

> [!NOTE]
> **Chiamare** al prompt dei comandi non ha alcun effetto quando viene usata all'esterno di un file batch o script.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
call [Drive:][Path]<FileName> [<BatchParameters>] [:<Label> [<Arguments>]]
```

## <a name="parameters"></a>Parametri

|           Parametro           |                                                                         Descrizione                                                                          |
|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:][<Path>]<FileName> | Specifica il percorso e nome del programma batch che si desidera chiamare. Il *FileName* parametro è obbligatorio e deve avere un'estensione. bat o cmd. |
|      \<BatchParameters>       |                                            Specifica le informazioni della riga di comando necessari per il programma batch.                                             |
|           :\<Etichetta >           |                                            Specifica l'etichetta da un controllo del programma batch a cui passare.                                             |
|         \<argomenti >          |                     Specifica le informazioni della riga di comando da passare alla nuova istanza del programma batch, iniziando in corrispondenza *: etichetta.*                     |
|              /?               |                                                             Visualizza la guida al prompt dei comandi.                                                             |

## <a name="batch-parameters"></a>Parametri di batch

I riferimenti agli argomenti di script batch ( **%0**, **%1**,...) sono elencati nelle tabelle seguenti.

**%** * in un batch dello script fa riferimento a tutti gli argomenti (ad esempio, **%1**, **%2**, **%3**...)

È possibile usare la sintassi facoltativi seguenti come le sostituzioni per i parametri di batch ( **%n**):

|Parametro di batch|Descrizione|
|---------------|-----------|
|%~1|Si espande **%1** e rimuove circostanti racchiusi tra virgolette ("").|
|%~f1|Si espande **%1** a un percorso completo.|
|%~d1|Si espande **%1** in solo una lettera di unità.|
|%~p1|Si espande **%1** solo in un percorso.|
|%~n1|Si espande **%1** a solo un nome di file.|
|%~x1|Si espande **%1** per un'estensione di file solo.|
|%~s1|Si espande **%1** a un percorso completo che contiene solo i nomi brevi.|
|%~a1|Si espande **%1** agli attributi di file.|
|%~t1|Si espande **%1** a data e ora del file.|
|%~z1|Si espande **%1** alle dimensioni del file.|
|%~$PATH:1|Cerca directory elencate nella variabile di ambiente PATH ed espande **%1** per specificare il nome completo della directory prima disponibile. Se il nome di variabile di ambiente non è definito o non è possibile trovare il file con la ricerca, quindi questo modificatore si espande in una stringa vuota.|

Nella tabella seguente viene illustrato come è possibile combinare i modificatori con i parametri di batch per ottenere risultati composti:

|Parametro di batch con modificatore|Descrizione|
|-----------------------------|-----------|
|%~dp1|Si espande **%1** una lettera di unità e un solo percorso.|
|%~nx1|Si espande **%1** a un nome di file e solo l'estensione.|
|%~dp$PATH:1|Cerca directory elencate nella variabile di ambiente PATH per **%1**e quindi si espande la lettera di unità e percorso della directory prima disponibile.|
|%~ftza1|Si espande **%1** per visualizzare l'output simile al **dir** comando.|

Negli esempi precedenti, **%1** e percorso può essere sostituito da altri valori validi. Il <strong>%~</strong> sintassi viene terminata da un numero di argomento valido. Il <strong>%~</strong> i modificatori non possono essere usati con **%\\** *.

## <a name="remarks"></a>Note

-   Utilizzo di parametri di batch

    Parametri batch possono contenere qualsiasi informazione che è possibile passare a un file batch, incluse le opzioni della riga di comando, i nomi di file, i parametri di batch **%0** attraverso **%9**, variabili e (ad esempio **% baud %** ).
-   Usando il *etichetta* parametro

    Usando **chiamare** con il *etichetta* parametro, creare un nuovo contesto di file batch e passare il controllo all'istruzione dopo l'etichetta specificata. La prima volta che viene rilevata la fine del file batch (vale a dire, dopo il passaggio all'etichetta), controllo viene restituito all'istruzione dopo il **chiamare** istruzione. La seconda volta che viene rilevata la fine del file batch, il batch viene terminato.
-   Usando pipe e i simboli di reindirizzamento

    Non usare pipe ( **|** ) e simboli di reindirizzamento ( **<** oppure **>** ) con **chiamare**.
-   Effettua una chiamata ricorsiva

    È possibile creare un file batch che chiama se stessa. Tuttavia, è necessario specificare una condizione di uscita. In caso contrario, i programmi di batch padre e figlio un ciclo infinito.
-   Utilizzo di estensioni di comando

    Se sono abilitate le estensioni dei comandi, **chiamare** accetta *etichetta* come destinazione della chiamata. La sintassi corretta è come segue:

    `call :\<Label> <Arguments>`

## <a name="BKMK_examples"></a>Esempi

Per eseguire il programma bat da un altro programma batch, digitare il comando seguente nel programma batch padre:
```
call checknew
```
Se il programma batch accetta due parametri di batch e si vuole poter passare tali parametri a bat, digitare il comando seguente nel programma batch padre:
```
call checknew %1 %2
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)