---
title: setpeercachingflags Bitsadmin
description: Windows Commands argomento for **BITSAdmin setpeercachingflags**, che imposta i flag che determinano se i file del processo possono essere memorizzati nella cache e serviti ai peer e se il processo può scaricare il contenuto dai peer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b4a7807975fb46440301e30b1fdbd01784d7c85
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122770"
---
# <a name="bitsadmin-setpeercachingflags"></a>setpeercachingflags Bitsadmin

Imposta i flag che determinano se i file del processo possono essere memorizzato nella cache e forniti a colleghi e se il processo è possibile scaricare contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setpeercachingflags <job> <value>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |
| value | Un Unsigned Integer, tra cui:<ul><li>**1.** il processo può scaricare il contenuto dai peer.</li><li>**2.** i file del processo possono essere memorizzati nella cache e serviti ai peer.</li></ul> |

## <a name="examples"></a>Esempi

Nell'esempio seguente vengono impostati i flag per il processo denominato *myDownloadJob*, che consente di scaricare il contenuto dai peer.

```
C:\>bitsadmin /setpeercachingflags myDownloadJob 1
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)