---
title: bitsadmin setpeercachingflags
description: Argomento di riferimento per il comando Bitsadmin setpeercachingflags, che imposta i flag che determinano se i file del processo possono essere memorizzati nella cache e serviti ai peer e se il processo può scaricare il contenuto dai peer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b66b169c38ac050ecaaf6546365547148faa9cf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717264"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags

Imposta i flag che determinano se i file del processo possono essere memorizzato nella cache e forniti a colleghi e se il processo è possibile scaricare contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setpeercachingflags <job> <value>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| value | Un Unsigned Integer, tra cui:<ul><li>**1.** il processo può scaricare il contenuto dai peer.</li><li>**2.** i file del processo possono essere memorizzati nella cache e serviti ai peer.</li></ul> |

## <a name="examples"></a>Esempi

Per consentire al processo denominato *myDownloadJob* di scaricare il contenuto dai peer:

```
bitsadmin /setpeercachingflags myDownloadJob 1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
