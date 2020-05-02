---
title: bitsadmin getnotifyinterface
description: Argomento di riferimento per il comando Bitsadmin getnotifyinterface, che determina se un altro programma ha registrato un'interfaccia di callback COM per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2158759067010292ca213f97014857354247b9c7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717733"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

Determina se un altro programma ha registrato un'interfaccia di callback COM (interfaccia Notify) per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getnotifyinterface <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

#### <a name="output"></a>Output

L'output per questo comando Visualizza, **registrato** o **annullato la registrazione**.

> [!NOTE]
> Non è possibile determinare il programma che ha registrato l'interfaccia di callback.

## <a name="examples"></a>Esempi

Per recuperare l'interfaccia Notify per il processo denominato *myDownloadJob*:

```
bitsadmin /getnotifyinterface myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
