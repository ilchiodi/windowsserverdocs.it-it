---
title: bitsadmin resume
description: Windows Commands argomento for **BITSAdmin Resume**, che attiva un processo nuovo o sospeso nella coda di trasferimento.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a3f464ba00c5cc233c42a40c063372dc0d584e9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849754"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

Attiva un processo nuovo o sospeso nella coda di trasferimento.

## <a name="syntax"></a>Sintassi

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente riprende il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)