---
title: bitsadmin peercaching e getconfigurationflags
description: Argomento di riferimento per il comando Bitsadmin peer caching e getconfigurationflags, che ottiene i flag di configurazione che determinano se il computer fornisce contenuto ai peer e se può scaricare il contenuto dai peer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62b6848dec30a9a9fef401b1b2372605dbb9934a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717328"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>bitsadmin peercaching e getconfigurationflags

Ottiene i flag di configurazione che determinano se il computer fornisce contenuto ai peer e se può scaricare il contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /peercaching /getconfigurationflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per ottenere i flag di configurazione per il processo denominato *myDownloadJob*:

```
bitsadmin /peercaching /getconfigurationflags myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)

- [comando Bitsadmin peer caching](bitsadmin-peercaching.md)
