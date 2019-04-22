---
title: prompt
description: Informazioni su come personalizzare il prompt dei comandi.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 320e12fd30deda30ccc0da1ad6e5bea6f9a19d8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818442"
---
# <a name="prompt"></a>prompt



Prompt dei comandi Cmd.exe viene modificato. Se utilizzata senza parametri, **prompt dei comandi** Reimposta il prompt dei comandi per l'impostazione predefinita, che è la lettera di unità corrente e directory seguito dal segno di maggiore (**>**).

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
prompt [<Text>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Text>|Specifica il testo e le informazioni che si desidera includere nel prompt dei comandi.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

È possibile personalizzare il prompt dei comandi per visualizzare il testo desiderato, incluse informazioni quali il nome della directory corrente, l'ora e data e il numero di versione di Microsoft Windows.

Nella tabella seguente sono elencate le combinazioni di caratteri che è possibile includere anziché o in aggiunta, uno o più stringhe di caratteri nel *testo* parametro. L'elenco include una breve descrizione del testo o le informazioni che ogni combinazione di caratteri aggiunge al prompt dei comandi.  
|Carattere|Descrizione|
|---------|-----------|
|$q|= (segno di uguale)|
|$$|$ (segno di dollaro)|
|$t|Ora corrente|
|$d|Data corrente|
|$p|Percorso e l'unità corrente|
|$v|Numero di versione di Windows|
|$n|Unità corrente|
|$g|> (segno di maggiore di)|
|$l|< (segno di minore di)|
|$b|| (barra verticale)|
|$_|IMMETTERE CON AVANZAMENTO DI RIGA|
|$e|Codice di escape ANSI (codice di 27)|
|$h|Carattere backspace (per eliminare un carattere che è stata scritta nella riga di comando)|
|$un|& (e commerciale)|
|$c|((parentesi)|
|$f|) (parentesi)|
|$s|Spazio|

Quando sono abilitate le estensioni dei comandi (vale a dire l'impostazione predefinita) il **prompt dei comandi** comando supporta i caratteri di formattazione seguenti:  

|Carattere|Descrizione|
|---------|-----------|
|$+|Zero o più segno (**+**) caratteri, a seconda la profondità del **pushd** stack directory (un carattere per ogni livello push).|
|$m|Il nome remoto associato con la lettera di unità corrente o una stringa vuota se l'unità corrente non è un'unità di rete.|

Se si include il **$p** caratteri nel parametro di testo viene letto il disco dopo ogni comando, per determinare l'unità corrente e il percorso, immesso. Ciò può richiedere tempo aggiuntivo, in particolare per le unità disco floppy.

## <a name="BKMK_examples"></a>Esempi

Per impostare un prompt dei comandi di due righe con la data e l'ora corrente per la prima riga e il segno di maggiore nella riga successiva, digitare:
```
prompt $d$s$s$t$_$g 
```
Il prompt dei comandi viene modificato come indicato di seguito, in cui la data e ora sono corrente:
```
Fri 06/01/2007  13:53:28.91
>
```
Per impostare il prompt dei comandi da visualizzare sotto forma di freccia (`-->`), tipo:
```
prompt --$g
```
Per modificare manualmente il prompt dei comandi per l'impostazione predefinita (l'unità corrente e il percorso seguito dal segno di maggiore), digitare:
```
prompt $p$g
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)