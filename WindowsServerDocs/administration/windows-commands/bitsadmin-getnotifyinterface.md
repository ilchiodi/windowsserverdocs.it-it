---
title: bitsadmin getnotifyinterface
description: Windows Commands Topic for **BITSAdmin getnotifyinterface**, che determina se un altro programma ha registrato un'interfaccia di callback com per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5eb5aee42446c70f16fd6785a3645f42c1987e4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850574"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

Determina se un altro programma ha registrato un'interfaccia di callback COM (interfaccia Notify) per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getnotifyinterface <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

#### <a name="output"></a>Output

L'output per questo comando Visualizza, **registrato** o **annullato la registrazione**.

> [!NOTE]
> Non è possibile determinare il programma che ha registrato l'interfaccia di callback.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperata l'interfaccia Notify per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getnotifyinterface myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)