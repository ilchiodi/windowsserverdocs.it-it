---
title: setcustomheaders Bitsadmin
description: Windows Commands Topic for **BITSAdmin setcustomheaders**, che aggiunge un'intestazione HTTP personalizzata a una richiesta GET.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5b1a28f03815a22a3f8d10b2c3d1d4a3a2ae635
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123022"
---
# <a name="bitsadmin-setcustomheaders"></a>setcustomheaders Bitsadmin

Aggiungere un'intestazione HTTP personalizzata a una richiesta GET inviata a un server HTTP.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setcustomheaders <job> <header1> <header2> <...>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |
| `<header1> <header2>` e così via | Intestazioni personalizzate per il processo. |

## <a name="examples"></a>Esempi

Nell'esempio seguente aggiunge un'intestazione HTTP personalizzata per il processo denominato *myDownloadJob*. Per altre informazioni sulle richieste GET, vedere definizioni di [Metodo](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3) e [definizioni dei campi di intestazione](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

```
C:\>bitsadmin /setcustomheaders myDownloadJob accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)