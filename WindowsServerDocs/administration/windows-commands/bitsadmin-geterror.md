---
title: bitsadmin geterror
description: Argomento di riferimento per il comando Bitsadmin GetError, che recupera informazioni dettagliate sugli errori per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7275bb813ef65f304cf8111c5a1ee387b7528e5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718014"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror

Recupera informazioni dettagliate sugli errori per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /geterror <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare le informazioni sull'errore per il processo denominato *myDownloadJob*:

```
bitsadmin /geterror myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
