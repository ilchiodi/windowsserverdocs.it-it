---
title: bitsadmin getdisplayname
description: Windows Commands argomento per **BITSAdmin GetDisplayName**, che recupera il nome visualizzato del processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6944dc2b7a63ca986fb285d26796f350c1052295
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850714"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname

Recupera il nome visualizzato del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getdisplayname <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperato il nome visualizzato per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getdisplayname myDownloadJob
```

## <a name="additional-references"></a>Altri riferimenti

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)