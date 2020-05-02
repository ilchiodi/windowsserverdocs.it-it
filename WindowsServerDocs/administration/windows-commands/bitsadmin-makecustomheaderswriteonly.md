---
title: bitsadmin makecustomheaderswriteonly
description: Argomento di riferimento per il comando Bitsadmin makecustomheaderswriteonly, che rende di sola scrittura le intestazioni HTTP personalizzate di un processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 2aeab7e0ee7797b3e0be7be1156920f3bafc84dc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717409"
---
# <a name="bitsadmin-makecustomheaderswriteonly"></a>bitsadmin makecustomheaderswriteonly

Rendere di sola scrittura le intestazioni HTTP personalizzate di un processo.

> [!IMPORTANT]
> Questa azione non può essere annullata.

## <a name="syntax"></a>Sintassi

```
bitsadmin /makecustomheaderswriteonly <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per rendere le intestazioni HTTP personalizzate di sola scrittura per il processo denominato *myDownloadJob*:

```
bitsadmin /makecustomheaderswriteonly myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
