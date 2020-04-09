---
title: bitsadmin geterrorcount
description: Windows Commands Topic for Bitsadmin GetErrorCount, che recupera un conteggio del numero di volte in cui il processo specificato ha generato un errore temporaneo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1711781dd416311e45874b7c611f3f8f42a06854
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850694"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount

Recupera un conteggio del numero di volte in cui il processo specificato ha generato un errore temporaneo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /geterrorcount <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono recuperate le informazioni sul conteggio degli errori per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)