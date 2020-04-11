---
title: bitsadmin reset
description: Windows Commands argomento for **BITSAdmin Reset**, che annulla tutti i processi nella coda di trasferimento di proprietà dell'utente corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b73bd3b9c66b24330a0f9444836b9c8bd1730722
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123087"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Annulla tutti i processi nella coda di trasferimento di proprietà dell'utente corrente. > Non è possibile reimpostare i processi creati dal sistema locale. Al contrario, è necessario essere un amministratore e usare l'utilità di pianificazione per pianificare questo comando come attività usando le credenziali del sistema locale.

> [!NOTE]
> In BITSAdmin 1,5 e versioni precedenti, se si dispone dei privilegi di amministratore, l'opzione/reset cancellerà tutti i processi nella coda. Inoltre, l'opzione/ALLUSERS non è supportata.

## <a name="syntax"></a>Sintassi

```
bitsadmin /reset [/allusers]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| /allusers | Facoltativa. Annulla tutti i processi nella coda di proprietà dell'utente corrente. Per utilizzare questo parametro, è necessario disporre dei privilegi di amministratore. |

## <a name="examples"></a>Esempi

Nell'esempio seguente Annulla tutti i processi nella coda di trasferimento per l'utente corrente.

```
C:\>bitsadmin /reset
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)