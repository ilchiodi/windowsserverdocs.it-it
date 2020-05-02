---
title: bitsadmin getreplydata
description: Argomento di riferimento per il comando Bitsadmin getreplydata, che consente di recuperare i dati di caricamento e risposta del server in formato esadecimale per il processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea2a82403fe05776abbbf65e87a4b6e72c8767b8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717622"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

Recupera i dati di caricamento/risposta del server in formato esadecimale per il processo.

> [!NOTE]
> Questo comando non è supportato da BITS 1,2 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getreplydata <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare i dati di caricamento-risposta per il processo denominato *myDownloadJob*:

```
bitsadmin /getreplydata myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
