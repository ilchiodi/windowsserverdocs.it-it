---
title: Converti dinamica
description: Argomento dei comandi di Windows per Convert Dynamic, che converte un disco di base in un disco dinamico.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ad84cf2ecb6808b68110187b52f3fc13590b491
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847324"
---
# <a name="convert-dynamic"></a>Converti dinamica

Converte un disco di base in un disco dinamico.

Per istruzioni su come usare questo comando, vedere [modificare un disco di base in un disco dinamico](https://go.microsoft.com/fwlink/?LinkId=207047) (https://go.microsoft.com/fwlink/?LinkId=207047).

## <a name="syntax"></a>Sintassi

```
convert dynamic [noerr]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Tutte le partizioni esistenti sul disco di base diventano volumi semplici.
-   Per eseguire questa operazione, è necessario selezionare un disco di base. Usare il comando **Seleziona disco** per selezionare un disco di base e spostare lo stato attivo a esso.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per convertire un disco di base in un disco dinamico, digitare:
```
convert dynamic
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

