---
title: bitsadmin gettype
description: Argomento di riferimento per il comando Bitsadmin GetType, che consente di recuperare il tipo di processo del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 151f9b8e81229a666111ebcd20f060d84160445a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717472"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype

Recupera il tipo di processo del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /gettype <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

#### <a name="output"></a>Output

I valori di output restituiti possono essere:

| Type | Description |
| --------------- | ----------- |
| Download | Il processo è un download. |
| Caricamento | Il processo è un caricamento. |
| Caricamento-risposta | Il processo è di caricamento-risposta. |
| Sconosciuto | Il tipo del processo è sconosciuto. |

## <a name="examples"></a>Esempi

Per recuperare il tipo di processo per il processo denominato *myDownloadJob*:

```
bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
