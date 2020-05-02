---
title: bitsadmin geterrorcount
description: Argomento di riferimento per il comando Bitsadmin GetErrorCount, che recupera un conteggio del numero di volte in cui il processo specificato ha generato un errore temporaneo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 516bd02ed296a2eba75e174c6f084926bde63e90
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718003"
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
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare le informazioni sul numero di errori per il processo denominato *myDownloadJob*:

```
bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
