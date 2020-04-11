---
title: bitsadmin setdisplayname
description: Windows Commands Topic for **BITSAdmin sedisplayname**, che imposta il nome visualizzato del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b1086903dd130392800f325c451bb4750fbf8fa
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123009"
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
| lavoro | Nome visualizzato o GUID del processo. |
| display_name | Testo utilizzato come nome visualizzato per il processo specifico. |

## <a name="examples"></a>Esempi

Nell'esempio seguente il nome visualizzato del processo viene impostato su *myDownloadJob*.

```
C:\>bitsadmin /setdisplayname myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)