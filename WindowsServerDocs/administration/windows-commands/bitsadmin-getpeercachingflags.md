---
title: bitsadmin getpeercachingflags
description: Argomento di riferimento per il comando Bitsadmin getpeercachingflags, che consente di recuperare i flag che determinano se i file del processo possono essere memorizzati nella cache e serviti ai peer e se BITS è in grado di scaricare il contenuto per il processo da peer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f30eead56958af3cd0fb0d91f6ee2bf9f79fdc4e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717685"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera i flag che determinano se i file del processo possono essere memorizzati nella cache e serviti ai peer e se BITS è in grado di scaricare il contenuto per il processo da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getpeercachingflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare i flag per il processo denominato *myDownloadJob*:

```
bitsadmin /getpeercachingflags myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
