---
title: bitsadmin getowner
description: Windows Commands argomento per Bitsadmin **GetOwner**, che recupera il proprietario del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e622c3759c9ec20867c693539c4481c70aa4f26
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850564"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

Consente di visualizzare il nome visualizzato o il GUID del proprietario del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getowner <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene visualizzato il proprietario del processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getowner myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)