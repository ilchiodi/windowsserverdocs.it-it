---
title: Data
description: Argomento di riferimento per date, che Visualizza o imposta la data di sistema. Se utilizzata senza parametri,
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7bcdb35579ac86b4ec7f9c7c639cf905f6a05fa
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716808"
---
# <a name="date"></a>Data

Visualizza o imposta la data di sistema. Se utilizzata senza parametri, **Data** Visualizza l'impostazione corrente della data di sistema e viene richiesto di immettere una nuova data.



## <a name="syntax"></a>Sintassi

```
date [/t | <Month-Day-Year>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> mese-giorno-anno|Imposta la data specificata, in cui *mese* è il mese (una o due cifre), *giorno* è il giorno (uno o due cifre), e *anno* è l'anno (due o quattro cifre).|
|/t|Visualizza la data corrente senza richiedere una nuova data.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Per modificare la data corrente, è necessario disporre di credenziali amministrative.
-   È necessario separare i valori per *mese*, *giorno*, e *anno* con punti (.), trattini (-), o una barra (/) vengono contrassegnate.
-   Valido *mese* i valori sono da 1 a 12.
-   Valido *giorno* i valori sono 1 e 31.
-   Valido *anno* i valori sono da 00 a 99 o 1980 e 2099. Se si utilizzano due cifre, i valori compresi tra 80 e 99 corrispondono agli anni 1980 e 1999.

## <a name="examples"></a>Esempi

Se sono abilitate le estensioni dei comandi, per visualizzare la data corrente del sistema, digitare:
```
date /t
```
Per modificare la data corrente del sistema a 3 agosto 2007, è possibile digitare uno dei seguenti:
```
date 08.03.2007
date 08-03-07
date 8/3/07
```
Per visualizzare la data corrente del sistema, seguita da una richiesta di immettere una nuova data, digitare:
```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yy)
```
Per mantenere la data corrente e tornare al prompt dei comandi, premere INVIO. Per modificare la data corrente, digitare la nuova data e quindi premere INVIO.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)