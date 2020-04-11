---
title: bitsadmin setpriority
description: Windows Commands argomento per **BITSAdmin sepriority**, che imposta la priorità del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9348680a61649b938267b3277de9aa5aa521361f
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122762"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

Imposta la priorità del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setpriority <job> <priority>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |
| priority | Imposta la priorità del processo, tra cui:<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |

## <a name="examples"></a>Esempi

Nell'esempio seguente imposta la priorità per il processo denominato *myDownloadJob* Normal.

```
C:\>bitsadmin /setpriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)