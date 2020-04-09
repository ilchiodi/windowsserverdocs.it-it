---
title: gettemporaryname Bitsadmin
description: Windows Commands argomento per **BITSAdmin gettemporaryname**, che segnala il nome file temporaneo del file specificato all'interno del processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c331ecf12cb02d34c76692158c79eafbe5691c5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850454"
---
# <a name="bitsadmin-gettemporaryname"></a>gettemporaryname Bitsadmin

Restituisce il nome del file temporaneo del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /gettemporaryname <job> <file_index>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |
| file_index | Inizia da 0. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene riportato il nome file temporaneo del file 2 per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /gettemporaryname myDownloadJob 1
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)