---
title: bitsadmin geterror
description: Argomento i comandi di Windows per **bitsadmin geterror** -estratto di informazioni dettagliate sull'errore per il processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10a3373c0c8f290ff1f5f26ef38531fbc7745890
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889972"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror



Recupera le informazioni di errore dettagliato per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetError <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera le informazioni sull'errore per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetError myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)