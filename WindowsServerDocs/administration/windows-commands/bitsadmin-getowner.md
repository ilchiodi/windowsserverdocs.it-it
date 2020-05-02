---
title: bitsadmin getowner
description: Argomento di riferimento per il comando Bitsadmin GetOwner, che recupera il proprietario del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a345ea47232f1f4d6340e1341747c9dad92382b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717713"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

Consente di visualizzare il nome visualizzato o il GUID del proprietario del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getowner <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per visualizzare il proprietario del processo denominato *myDownloadJob*:

```
bitsadmin /getowner myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
