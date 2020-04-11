---
title: bitsadmin geterrorcount
description: Windows Commands Topic for **BITSAdmin GetErrorCount**, che recupera un conteggio del numero di volte in cui il processo specificato ha generato un errore temporaneo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef0bf043517d4edfa8d72888746ca5d9c92ecc21
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123131"
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