---
title: getvalidationstate Bitsadmin
description: Windows Commands Topic for **BITSAdmin getvalidationstate**, che segnala lo stato di convalida del contenuto del file specificato all'interno del processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ada3f1f-9967-4262-9d22-ed641e23f516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52d7d983cc7858607c350483ed81223d107cee25
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850434"
---
# <a name="bitsadmin-getvalidationstate"></a>getvalidationstate Bitsadmin

Segnala lo stato di convalida del contenuto del file specificato all'interno del processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getvalidationstate <job> <file_index>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |
| file_index | Inizia da 0. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene ottenuto lo stato di convalida del contenuto del file 2 nel processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getvalidationstate myDownloadJob 1
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)