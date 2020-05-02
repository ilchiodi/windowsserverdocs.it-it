---
title: bitsadmin gethttpmethod
description: Argomento di riferimento per il comando Bitsadmin GetHttpMethod, che ottiene il verbo HTTP da usare con il processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a458322a5ace69df74df054a537a7365da9e7329
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717887"
---
# <a name="bitsadmin-gethttpmethod"></a>bitsadmin gethttpmethod

Ottiene il verbo HTTP da utilizzare con il processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /gethttpmethod <Job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare il verbo HTTP da usare con il processo denominato *myDownloadJob*:

```
bitsadmin /gethttpmethod myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
