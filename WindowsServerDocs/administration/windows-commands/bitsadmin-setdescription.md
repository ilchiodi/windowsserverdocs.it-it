---
title: bitsadmin setdescription
description: Windows Commands argomento per **BITSAdmin**, che imposta la descrizione del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b62e6b030c23c475418cd6f2c63f04edba1acff
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123012"
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
| lavoro | Nome visualizzato o GUID del processo. |
| description | Testo utilizzato per descrivere il processo. |

## <a name="examples"></a>Esempi

Nell'esempio seguente viene recuperata la descrizione per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /setdescription myDownloadJob music_downloads
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)