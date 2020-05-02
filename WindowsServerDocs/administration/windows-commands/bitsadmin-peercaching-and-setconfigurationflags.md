---
title: bitsadmin peercaching e setconfigurationflags
description: Argomento di riferimento per il comando Bitsadmin peer caching e setconfigurationflags, che imposta i flag di configurazione che determinano se il computer può gestire il contenuto ai peer e se può scaricare il contenuto dai peer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c3ce69ce7a372311ce0c30e9b3a391ea33f45ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717232"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin peercaching e setconfigurationflags

Imposta i flag di configurazione che determinano se il computer può gestire il contenuto ai peer e se può scaricare il contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /peercaching /setconfigurationflags <job> <value>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |
| value | Unsigned Integer con l'interpretazione seguente per i bit nella rappresentazione binaria:<ul><li>Per consentire il download dei dati del processo da un peer, impostare il bit meno significativo.</li><li>Per consentire ai dati del processo di essere serviti ai peer, impostare il secondo bit da destra.</li></ul>|

## <a name="examples"></a>Esempi

Per specificare i dati del processo da scaricare dai peer per il processo denominato *myDownloadJob*:

```
bitsadmin /peercaching /setconfigurationflags myDownloadJob 1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)

- [comando Bitsadmin peer caching](bitsadmin-peercaching.md)
