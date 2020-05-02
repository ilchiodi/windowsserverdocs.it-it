---
title: bitsadmin setcustomheaders
description: Argomento di riferimento per il comando Bitsadmin setcustomheaders, che aggiunge un'intestazione HTTP personalizzata a una richiesta GET.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 92728f8d63a22cf9d13d6c02a69359583a9fc5cc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719329"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders

Aggiungere un'intestazione HTTP personalizzata a una richiesta GET inviata a un server HTTP. Per altre informazioni sulle richieste GET, vedere definizioni di [Metodo](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3) e [definizioni dei campi di intestazione](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

## <a name="syntax"></a>Sintassi

```
bitsadmin /setcustomheaders <job> <header1> <header2> <...>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| `<header1> <header2>`E così via | Intestazioni personalizzate per il processo. |

## <a name="examples"></a>Esempi

Per aggiungere un'intestazione HTTP personalizzata per il processo denominato *myDownloadJob*:

```
bitsadmin /setcustomheaders myDownloadJob accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
