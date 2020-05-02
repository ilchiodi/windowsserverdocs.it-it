---
title: convert gpt
description: Argomento di riferimento per il comando Convert GPT, che converte un disco di base vuoto con lo stile di partizione MBR (master boot record) in un disco di base con lo stile di partizione GPT (tabella di partizione GUID).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25b28473716037235a70e05835e23790f93164a1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720772"
---
# <a name="convert-gpt"></a>convert gpt

Converte un disco di base vuoto con lo stile di partizione MBR (master boot record) in un disco di base con lo stile di partizione GPT (tabella di partizione GUID). Per eseguire questa operazione, è necessario selezionare un disco MBR di base. Usare il [comando Seleziona disco](select-disk.md) per selezionare un disco di base e spostare lo stato attivo a esso.

> [!IMPORTANT]
> Il disco deve essere vuoto per convertirlo in un disco di base. Eseguire il backup dei dati e quindi eliminare tutte le partizioni o i volumi prima di convertire il disco. Le dimensioni minime del disco necessarie per la conversione in GPT sono pari a 128 megabyte.

> [!NOTE]
> Per istruzioni su come usare questo comando, vedere [modificare un disco di record di avvio principale in un disco della tabella di partizione GUID](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725671(v=ws.11)).

## <a name="syntax"></a>Sintassi

```
convert gpt [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per convertire un disco di base dallo stile di partizione MBR nello stile di partizione GPT, digitare:

```
convert gpt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Convert (comando)](convert.md)
