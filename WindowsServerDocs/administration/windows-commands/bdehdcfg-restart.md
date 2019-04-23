---
title: bdehdcfg riavvio
description: Argomento i comandi di Windows per bdehdcfg restart - indica bdehdcfg che il computer deve essere riavviato dopo la preparazione dell'unità.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f361db8fdf33bd414556575de75241f7dbd9327
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879462"
---
# <a name="bdehdcfg-restart"></a>bdehdcfg: restart



Informa lo strumento da riga di comando Bdehdcfg che il computer deve essere riavviato dopo la preparazione dell'unità. Per un esempio di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -restart
```

### <a name="parameters"></a>Parametri

Questo comando non accetta parametri aggiuntivi.

## <a name="remarks"></a>Note

Se altri utenti sono connessi al computer e il **quiet** comando non è specificato, verrà visualizzato un prompt dei comandi per verificare che il computer deve essere riavviato.

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **riavviare** comando.
```
bdehdcfg -target default -restart
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)