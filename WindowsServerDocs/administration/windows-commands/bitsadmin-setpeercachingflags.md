---
title: setpeercachingflags Bitsadmin
description: Argomento dei comandi di Windows per **BITSAdmin setpeercachingflags** -imposta i flag che determinano se i file del processo possono essere memorizzati nella cache e serviti ai peer e se il processo può scaricare il contenuto dai peer.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 147f28268f1b4dd6dfb40cff85f073feabbc35a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380456"
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
|Value|Il valore è un intero senza segno con l'interpretazione seguente per i bit nella rappresentazione binaria.</br>1-il processo può scaricare il contenuto dai peer.</br>2-i file del processo possono essere memorizzati nella cache e serviti ai peer.|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta i flag per il processo denominato *myJob* che consente di scaricare contenuto da peer.
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)