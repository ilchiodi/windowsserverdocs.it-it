---
title: bitsadmin setdisplayname
description: Argomento di riferimento per il comando Bitsadmin sedisplayname, che imposta il nome visualizzato del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 382cb2f20f0374c2d2787c4c3d88670b4f7260cd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719384"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname

Imposta il nome visualizzato per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setdisplayname <job> <display_name>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| display_name | Testo utilizzato come nome visualizzato per il processo specifico. |

## <a name="examples"></a>Esempi

Per impostare il nome visualizzato per il processo su *myDownloadJob*:

```
bitsadmin /setdisplayname myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
