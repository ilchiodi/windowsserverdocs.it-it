---
title: bitsadmin getcreationtime
description: Argomento di riferimento per il comando Bitsadmin GetCreationTime, che consente di recuperare l'ora di creazione per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc6ca5ad23730e9f57d58e069e0a2daf961930e8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718109"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime

Recupera l'ora di creazione per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getcreationtime <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare l'ora di creazione per il processo denominato *myDownloadJob*:

```
bitsadmin /getcreationtime myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
