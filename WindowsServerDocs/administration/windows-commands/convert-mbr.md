---
title: convert mbr
description: Argomento di riferimento per il comando Convert MBR, che converte un disco di base vuoto con lo stile di partizione GPT (tabella di partizione GUID) in un disco di base con lo stile di partizione MBR (master boot record).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a635a4c0-af73-4330-b021-51d483424537
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 178384ff63267c6ca22069f49b980a316b7695aa
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720762"
---
# <a name="convert-mbr"></a>convert mbr

Converte un disco di base vuoto con lo stile di partizione GPT (tabella di partizione GUID) in un disco di base con lo stile di partizione MBR (master boot record). Per eseguire questa operazione, è necessario selezionare un disco di base. Usare il [comando Seleziona disco](select-disk.md) per selezionare un disco di base e spostare lo stato attivo a esso.

> [!IMPORTANT]
> Il disco deve essere vuoto per convertirlo in un disco di base. Eseguire il backup dei dati e quindi eliminare tutte le partizioni o i volumi prima di convertire il disco.

> [!NOTE]
> Per istruzioni su come usare questo comando, vedere [modificare un disco della tabella di partizione GUID in un disco di record di avvio principale](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725797(v=ws.11)).

## <a name="syntax"></a>Sintassi

```
convert mbr [noerr]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |

## <a name="examples"></a>Esempi

Per convertire un disco di base dallo stile di partizione GPT allo stile di partizione MBR, digitare>:

```
convert mbr
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Convert (comando)](convert.md)
