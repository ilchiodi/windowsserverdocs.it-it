---
title: bitsadmin removeclientcertificate
description: Argomento di riferimento per il comando Bitsadmin removeclientcertificate, che rimuove il certificato client dal processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 513830f6048f78aa528fa22cb590571e718452c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717062"
---
# <a name="bitsadmin-removeclientcertificate"></a>bitsadmin removeclientcertificate

Rimuove il certificato del client dal processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /removeclientcertificate <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per rimuovere il certificato client dal processo denominato *myDownloadJob*:

```
bitsadmin /removeclientcertificate myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
