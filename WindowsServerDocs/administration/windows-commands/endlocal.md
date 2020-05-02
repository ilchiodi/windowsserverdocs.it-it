---
title: endlocal
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f007a9ec1e86093192630011c5197740dfefe922
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719372"
---
# <a name="endlocal"></a>endlocal



Termina la localizzazione delle modifiche all'ambiente in un file batch e ripristinare i valori delle variabili di ambiente prima della corrispondente **setlocal** comando è stato eseguito.



## <a name="syntax"></a>Sintassi

```
endlocal
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Il **endlocal** comando non ha alcun effetto all'esterno di un file batch o script.
-   È implicita **endlocal** comando alla fine di un file batch.
-   Se sono abilitate le estensioni dei comandi (estensioni di comando sono abilitate per impostazione predefinita), il **endlocal** comando Ripristina lo stato delle estensioni (vale a dire, abilitato o disabilitato) in vigore prima della corrispondente **setlocal** comando è stato eseguito.

> [!NOTE]
> Per ulteriori informazioni sull'abilitazione e disabilitazione delle estensioni del comando, vedere [Cmd](cmd.md).

## <a name="examples"></a>Esempi

È possibile localizzare le variabili di ambiente in un file batch. Ad esempio, il programma seguente viene avviato il programma batch superapp sulla rete, indirizza l'output in un file e viene visualizzato il file nel blocco note:
```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)