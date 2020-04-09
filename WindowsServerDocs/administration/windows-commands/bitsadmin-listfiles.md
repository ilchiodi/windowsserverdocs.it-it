---
title: bitsadmin listfiles
description: Windows Commands Topic for **BITSAdmin ListFiles**, che elenca i file nel processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad0d1eaa-3bd8-45e5-8f72-4da7366f0d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1af11f7876a3d1cd36aa38c7ac26563c01e81ab5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850314"
---
# <a name="bitsadmin-listfiles"></a>bitsadmin listfiles

Elenca i file del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /listfiles <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente recupera l'elenco dei file per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /listfiles myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)