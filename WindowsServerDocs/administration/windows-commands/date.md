---
title: date
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e774f7bfabb9b574255691dd97d2cfff36f034e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877952"
---
# <a name="date"></a>date



Visualizza o imposta la data di sistema. Se utilizzata senza parametri, **Data** Visualizza l'impostazione corrente della data di sistema e viene richiesto di immettere una nuova data.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
date [/t | <Month-Day-Year>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Mese-giorno-anno >|Imposta la data specificata, in cui *mese* è il mese (una o due cifre), *giorno* è il giorno (uno o due cifre), e *anno* è l'anno (due o quattro cifre).|
|/t|Visualizza la data corrente senza richiedere una nuova data.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per modificare la data corrente, è necessario disporre di credenziali amministrative.
-   È necessario separare i valori per *mese*, *giorno*, e *anno* con punti (.), trattini (-), o una barra (/) vengono contrassegnate.
-   Valido *mese* i valori sono da 1 a 12.
-   Valido *giorno* i valori sono 1 e 31.
-   Valido *anno* i valori sono da 00 a 99 o 1980 e 2099. Se si utilizzano due cifre, i valori compresi tra 80 e 99 corrispondono agli anni 1980 e 1999.

## <a name="BKMK_examples"></a>Esempi

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

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)