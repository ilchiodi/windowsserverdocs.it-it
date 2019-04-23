---
title: rem
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85c8a69bf21a386cd36e45bbca6dacd35aef2509
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847002"
---
# <a name="rem"></a>rem



Registra commenti (note) in un file batch o CONFIG. SYS. Se non viene specificato alcun commento, **rem** aggiunge spaziatura verticale.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
rem [<Comment>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Comment>|Specifica una stringa di caratteri da includere come commento.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Il **rem** comando consente di visualizzare i commenti sullo schermo. È necessario utilizzare il **echo in** comando nel batch o nel file CONFIG. File SYS per visualizzare i commenti sullo schermo.
-   Non è possibile utilizzare un carattere di reindirizzamento (**<** o **>**) o pipe (**|**) in un commento di file batch.
-   Sebbene sia possibile utilizzare **rem** senza un commento per aggiungere spaziatura verticale in un file batch, è inoltre possibile utilizzare le righe vuote. Righe vuote vengono ignorate durante l'elaborazione di un file batch.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene illustrato un file batch che utilizza la sezione Osservazioni per i commenti e per la spaziatura verticale:
```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause 
format b: /v chkdsk b: 
```
Per includere un commento esplicativo prima il **prompt** comando nel file CONFIG. SYS file, aggiungere le righe seguenti al file CONFIG. SYS:
```
rem Set prompt to indicate current directory
prompt $p$g
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)