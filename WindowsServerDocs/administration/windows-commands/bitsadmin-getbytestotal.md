---
title: bitsadmin getbytestotal
description: Argomento di riferimento per il comando Bitsadmin getbytestotal, che consente di recuperare le dimensioni del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f844e1d3689c42a2c533921797d15dbb946b551e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718157"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

Recupera le dimensioni del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare le dimensioni del processo denominato *myDownloadJob*:

```
bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
