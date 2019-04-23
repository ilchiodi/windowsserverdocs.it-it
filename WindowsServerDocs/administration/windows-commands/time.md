---
title: ora
description: Informazioni su come impostare e visualizzare l'ora di sistema.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5c1f43be98a19c4b150c247cc7fd48d62edeb5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861912"
---
# <a name="time"></a>ora



Visualizza o imposta l'ora di sistema. Se utilizzata senza parametri, **ora** Visualizza l'ora di sistema corrente e viene richiesto di immettere un'ora nuove.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<HH > [:\<MM > [:\<SS > [.\< NN >]]] [sto\|pm]|Imposta l'ora di sistema per la nuova ora specificata, in cui *HH* è espresso in ore (obbligatoria), *MM* in minuti e *SS* è espresso in secondi. *NN* può essere utilizzato per specificare i centesimi di secondo. Se **sono** o **pm** non è specificato, **ora** utilizza il formato di 24 ore per impostazione predefinita.|
|/t|Visualizza l'ora corrente senza richiedere una nuova ora.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per modificare l'ora corrente, è necessario disporre di credenziali amministrative.
-   È necessario separare i valori per *HH*, *MM*, e *SS* con due punti (:). *SS* e *NN* devono essere separati da un punto (.).
-   Valido *HH* i valori sono compresi tra 0 e 24.
-   Valido *MM* e *SS* i valori sono compresi tra 0 e 59.

## <a name="BKMK_examples"></a>Esempi

Se sono abilitate le estensioni dei comandi, per visualizzare l'ora di sistema corrente, digitare:
```
time /t
```
Per modificare l'ora di sistema corrente alle 17:30:00, digitare uno dei seguenti:
```
time 17:30:00
time 5:30 pm
```
Per visualizzare l'ora di sistema corrente, seguita da una richiesta di immettere una nuova ora, digitare:
```
The current time is: 17:33:31.35
Enter the new time:
```
Per mantenere l'ora corrente e tornare al prompt dei comandi, premere INVIO. Per modificare l'ora corrente, digitare la nuova ora e quindi premere INVIO.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)