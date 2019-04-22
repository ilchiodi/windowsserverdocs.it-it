---
title: getconfigurationflags e bitsadmin Caching
description: 'Argomento i comandi di Windows per **bitsadmin caching e getconfigurationflags** : Ottiene i flag di configurazione che determinano se il computer fornisce contenuto per peer e può scaricare contenuto da peer.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6afa39993cf90b2d71b6b681680c3b4e1fd9b56b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826352"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>getconfigurationflags e bitsadmin Caching



Ottiene i flag di configurazione che determinano se il computer fornisce contenuto per peer e può scaricare il contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /PeerCaching /GetConfigurationFlags <Job> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente ottiene i flag di configurazione per il processo denominato *myJob*.
```
C:\> Bitsadmin /PeerCaching /GetConfigurationFlags myJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)