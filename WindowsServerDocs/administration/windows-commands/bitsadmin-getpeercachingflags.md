---
title: getpeercachingflags Bitsadmin
description: 'Argomento dei comandi di Windows per **BITSAdmin getpeercachingflags** : recupera i flag che determinano se i file del processo possono essere memorizzati nella cache e serviti ai peer e se BITS è in grado di scaricare il contenuto per il processo da peer.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6b86214b5289a59e8db2ecff065ab3b8cd17007e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381442"
---
# <a name="bitsadmin-getpeercachingflags"></a>getpeercachingflags Bitsadmin

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera i flag che determinano se i file del processo possono essere memorizzati nella cache e serviti ai peer e se BITS è in grado di scaricare il contenuto per il processo da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetPeerCachingFlags <Job> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi
Nell'esempio seguente vengono recuperati i flag per il processo denominato *il mio lavoro*.

```
C:\>bitsadmin /GetPeerCachingFlags myJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)


