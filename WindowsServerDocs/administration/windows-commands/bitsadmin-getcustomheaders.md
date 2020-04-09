---
title: getcustomheaders Bitsadmin
description: Windows Commands Topic for **BITSAdmin getcustomheaders**, che consente di recuperare le intestazioni HTTP personalizzate dal processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80a3d1230fd541f0b986434ce373da4c8bb0c761
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850734"
---
# <a name="bitsadmin-getcustomheaders"></a>getcustomheaders Bitsadmin

Recupera le intestazioni HTTP personalizzate dal processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getcustomheaders <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono ottenute le intestazioni personalizzate per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getcustomheaders myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)