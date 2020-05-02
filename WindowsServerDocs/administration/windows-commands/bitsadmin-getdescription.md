---
title: bitsadmin getdescription
description: Argomento di riferimento per il comando Bitsadmin GetDescription, che consente di recuperare la descrizione del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec5fa9875ca9f669c2a43d58532d3e5e0770d550
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718075"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription

Recupera la descrizione del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getdescription <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare la descrizione per il processo denominato *myDownloadJob*:

```
bitsadmin /getdescription myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
