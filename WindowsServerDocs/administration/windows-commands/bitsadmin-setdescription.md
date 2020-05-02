---
title: bitsadmin setdescription
description: Argomento di riferimento per il comando Bitsadmin sedescription, che imposta la descrizione del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc76da7cbe348461a79984b8061767711e090da7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719302"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription

Imposta la descrizione per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setdescription <job> <description>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| description | Testo utilizzato per descrivere il processo. |

## <a name="examples"></a>Esempi

Per recuperare la descrizione per il processo denominato *myDownloadJob*:

```
bitsadmin /setdescription myDownloadJob music_downloads
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
