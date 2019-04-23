---
title: Bitsadmin getproxylist - recupera l'elenco di proxy per il processo specificato.
description: Argomento i comandi di Windows per **bitsadmin getproxylist** -recupera l'elenco di proxy per il processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8c3ffb1e425552cda5b14a00287817ace77a90f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840512"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera l'elenco di proxy per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetProxyList <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Il proxy è l'elenco dei server proxy da utilizzare. L'elenco è delimitato da virgole.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera l'elenco di proxy per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)