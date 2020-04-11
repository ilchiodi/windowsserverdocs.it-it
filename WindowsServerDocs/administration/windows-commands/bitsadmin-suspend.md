---
title: bitsadmin suspend
description: Argomento dei comandi di Windows per **BITSAdmin Suspend**, che sospende il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42ed83d4dbf8c3d982c5c186b440cf17997903c9
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123159"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sospende il processo specificato. Se il processo è stato sospeso per errore, è possibile usare l'opzione [BITSAdmin Resume](bitsadmin-resume.md) per riavviare il processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /suspend <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ---------- |
| Job | Nome visualizzato o GUID del processo. |

## <a name="example"></a>Esempio

Nell'esempio seguente viene sospeso il processo denominato *myDownloadJob*.


```
C:\>bitsadmin /suspend myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
