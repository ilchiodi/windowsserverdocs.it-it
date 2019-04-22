---
title: setpeercachingflags Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin setpeercachingflags** -imposta i flag che determinano se i file del processo possono essere memorizzato nella cache e forniti a colleghi e se il processo può scaricare contenuto da peer.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d50a6ccd83a6251808ca3d66437e52f641c60a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814252"
---
# <a name="bitsadmin-setpeercachingflags"></a>setpeercachingflags Bitsadmin



Imposta i flag che determinano se i file del processo possono essere memorizzato nella cache e forniti a colleghi e se il processo è possibile scaricare contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Value|Il valore è un intero senza segno con l'interpretazione seguente per i bit nella rappresentazione binaria.</br>1 - il processo può scaricare contenuto da peer.</br>2 - i file del processo possono essere memorizzato nella cache e forniti a colleghi.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta i flag per il processo denominato *myJob* che consente di scaricare contenuto da peer.
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)