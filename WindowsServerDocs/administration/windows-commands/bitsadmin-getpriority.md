---
title: Bitsadmin GetPriority
description: Windows Commands argomento per **BITSAdmin GetPriority**, che recupera la priorità del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: b27829a0fb852abb88c88a65e61e8d7693ca2df2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850544"
---
# <a name="bitsadmin-getpriority"></a>Bitsadmin GetPriority

Recupera la priorità del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="remarks"></a>Note

La priorità per questo comando può essere:

- **FOREGROUND**

- **ALTA**

- **NORMALE**

- **BASSO**

- **SCONOSCIUTO**

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperata la priorità per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
