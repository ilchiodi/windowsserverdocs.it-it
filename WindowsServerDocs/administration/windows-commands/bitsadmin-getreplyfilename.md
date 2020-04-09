---
title: bitsadmin getreplyfilename
description: Windows Commands Topic for **BITSAdmin getreplyfilename**, che ottiene il percorso del file che contiene la risposta di caricamento del server per il processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 541a6e60d641405b5da2e65fecbbbe87468c8702
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850494"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Ottiene il percorso del file che contiene la risposta di caricamento del server per il processo.

> [!NOTE]
> Questo comando non è supportato da BITS 1,2 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getreplyfilename <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |


## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperato il nome file della risposta di caricamento per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getreplyfilename myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)