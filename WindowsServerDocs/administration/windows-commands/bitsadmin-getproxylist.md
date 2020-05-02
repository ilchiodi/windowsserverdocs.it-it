---
title: "Bitsadmin getproxys: Recupera l'elenco di proxy per il processo specificato."
description: Argomento di riferimento per il comando GetProxy Bitsadmin, che consente di recuperare l'elenco di proxy per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a92703d83cc872204d3dc488c15d703dfd50a780
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717661"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera l'elenco delimitato da virgole dei server proxy da utilizzare per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare l'elenco di proxy per il processo denominato *myDownloadJob*:

```
bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
