---
title: getpeercachingflags Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin getpeercachingflags** -recupera i flag che determinano se i file del processo possono essere memorizzato nella cache e forniti a colleghi e BITS può scaricare il contenuto per il processo da peer.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eabcc5cacdcc5f7f4de7178b5afeff2acc89d7a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862812"
---
#<a name="bitsadmin-getpeercachingflags"></a>getpeercachingflags Bitsadmin

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera i flag che determinano se i file del processo possono essere memorizzato nella cache e forniti a colleghi e BITS può scaricare il contenuto per il processo da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi
Nell'esempio seguente recupera i flag per il processo denominato *myJob*.

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)


