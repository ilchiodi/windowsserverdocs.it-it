---
title: bitsadmin getfilestotal
description: Windows Commands Topic for **BITSAdmin getfilestotal**, che recupera il numero di file nel processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bad42a8bef57ca4c4a1411a12f20979e4a95d178
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850684"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal

Recupera il numero di file nel processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getfilestotal <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperato il numero di file inclusi nel processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getfilestotal myDownloadJob
```

## <a name="see-also"></a>Vedi anche

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
