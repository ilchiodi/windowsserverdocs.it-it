---
title: bitsadmin getnotifyflags
description: Argomento di riferimento per il comando Bitsadmin getnotifyflags, che consente di recuperare i flag di notifica per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36e4c3584b2e3be9c9985756aeaec08b40e74b0c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717770"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags

Recupera i flag di notifica per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getnotifyflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="remarks"></a>Osservazioni

Il processo può contenere uno o più dei flag di notifica seguenti:

| Flag | Descrizione |
| ----- | ----- |
| 0x001 | Generare un evento quando sono stati trasferiti tutti i file del processo. |
| 0x002 | Generare un evento quando si verifica un errore. |
| 0x004 | Disabilitare le notifiche. |
| 0x008 | Genera un evento quando il processo viene modificato o viene eseguito lo stato del trasferimento. |

## <a name="examples"></a>Esempi

Per recuperare i flag di notifica per il processo denominato *myDownloadJob*:

```
bitsadmin /getnotifyflags myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
