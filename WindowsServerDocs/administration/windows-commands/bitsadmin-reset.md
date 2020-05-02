---
title: bitsadmin reset
description: Argomento di riferimento per il comando di reimpostazione Bitsadmin, che annulla tutti i processi nella coda di trasferimento di proprietà dell'utente corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a6aea1d3cb0a89def1e23f42272bf0503022ac54
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717011"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Annulla tutti i processi nella coda di trasferimento di proprietà dell'utente corrente. Non è possibile reimpostare i processi creati dal sistema locale. Al contrario, è necessario essere un amministratore e usare l'utilità di pianificazione per pianificare questo comando come attività usando le credenziali del sistema locale.

> [!NOTE]
> Se si dispone di privilegi di amministratore in BITSAdmin 1,5 e versioni precedenti, l'opzione/reset cancellerà tutti i processi nella coda. Inoltre, l'opzione/ALLUSERS non è supportata.

## <a name="syntax"></a>Sintassi

```
bitsadmin /reset [/allusers]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| /allusers | Facoltativa. Annulla tutti i processi nella coda di proprietà dell'utente corrente. Per utilizzare questo parametro, è necessario disporre dei privilegi di amministratore. |

## <a name="examples"></a>Esempi

Per annullare tutti i processi nella coda di trasferimento per l'utente corrente.

```
bitsadmin /reset
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
