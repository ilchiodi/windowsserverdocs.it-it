---
title: bitsadmin info
description: Windows Commands argomento for **BITSAdmin info**, che visualizza le informazioni di riepilogo sul processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20b8358caba3e0c07b0c985cb24e8f7bde43b06c
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123110"
---
# <a name="bitsadmin-info"></a>bitsadmin info

Visualizza le informazioni di riepilogo sul processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |
| /verbose | Facoltativa. Fornisce informazioni dettagliate su ogni processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono recuperate le informazioni sul processo denominato *myDownloadJob*.

```
C:\>bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)