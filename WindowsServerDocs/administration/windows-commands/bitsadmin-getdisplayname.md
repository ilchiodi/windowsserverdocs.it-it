---
title: bitsadmin getdisplayname
description: Argomento di riferimento per il comando Bitsadmin GetDisplayName, che consente di recuperare il nome visualizzato del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f7c92fdb7c743c1a4c71f076764f5d1da2d95a6f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718023"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname

Recupera il nome visualizzato del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getdisplayname <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare il nome visualizzato per il processo denominato *myDownloadJob*:

```
bitsadmin /getdisplayname myDownloadJob
```

## <a name="additional-references"></a>Altri riferimenti

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
