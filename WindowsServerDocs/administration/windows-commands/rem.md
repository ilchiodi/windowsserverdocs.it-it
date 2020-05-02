---
title: rem
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d115548f15ff45087a771458062da8a3ef919eb3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722457"
---
# <a name="rem"></a>rem



Registra commenti (note) in un file batch o CONFIG. SYS. Se non viene specificato alcun commento, **rem** aggiunge spaziatura verticale.



## <a name="syntax"></a>Sintassi

```
rem [<Comment>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> Commenti|Specifica una stringa di caratteri da includere come commento.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Il **rem** comando consente di visualizzare i commenti sullo schermo. È necessario utilizzare il **echo in** comando nel batch o nel file CONFIG. File SYS per visualizzare i commenti sullo schermo.
-   Non è possibile utilizzare un carattere di reindirizzamento (**<** o **>**) o pipe (**|**) in un commento del file batch.
-   Sebbene sia possibile utilizzare **rem** senza un commento per aggiungere spaziatura verticale in un file batch, è inoltre possibile utilizzare le righe vuote. Righe vuote vengono ignorate durante l'elaborazione di un file batch.

## <a name="examples"></a>Esempi

Per visualizzare un file batch che usa le osservazioni per i commenti e la spaziatura verticale:
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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)