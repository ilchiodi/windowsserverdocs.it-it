---
title: bitsadmin getcreationtime
description: Windows Commands Topic for **BITSAdmin GetCreationTime**, che consente di recuperare l'ora di creazione per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd8f718e53cc44dc5f4c6f5ff09c9a5c201e0564
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850744"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime

Recupera l'ora di creazione per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getcreationtime <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperata l'ora di creazione per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getcreationtime myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)