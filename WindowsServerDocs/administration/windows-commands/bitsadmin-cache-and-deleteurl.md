---
title: Bitsadmin cache e deleteURL
description: Argomento di riferimento per la cache Bitsadmin e il comando deleteURL, che elimina tutte le voci della cache per l'URL specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 075c48e5c8c205cbbf3fe476260ec7909edcc3e6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718443"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>Bitsadmin cache e deleteURL

Elimina tutte le voci della cache per l'URL specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /deleteURL URL
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| URL | Uniform Resource Locator che identifica un file remoto. |

## <a name="examples"></a>Esempi

Per eliminare tutte le voci della `https://www.contoso.com/en/us/default.aspx`cache per:

```
bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx 
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando cache Bitsadmin](bitsadmin-cache.md)
