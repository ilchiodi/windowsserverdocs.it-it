---
title: setvalidationstate Bitsadmin
description: Windows Commands Topic for **BITSAdmin setvalidationstate**, che imposta lo stato di convalida del contenuto del file specificato all'interno del processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bec42ae926050cd21df594a38f1c441a40a527f
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122715"
---
# <a name="bitsadmin-setvalidationstate"></a>setvalidationstate Bitsadmin

Imposta lo stato di convalida del contenuto del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setvalidationstate <job> <file_index> <TRUE|FALSE>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ---------- |
| Job | Nome visualizzato o GUID del processo. |
| file_index | Inizia da 0. |
| TRUE o FALSE | **True** attiva la convalida del contenuto per il file specificato, mentre **false** lo disattiva. |

## <a name="examples"></a>Esempi

L'esempio seguente imposta lo stato di convalida del contenuto del file 2 su TRUE per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /setvalidationstate myDownloadJob 2 TRUE
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)