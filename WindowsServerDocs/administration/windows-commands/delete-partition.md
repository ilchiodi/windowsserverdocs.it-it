---
title: Elimina partizione
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf9f456ad6ab3010493154da843b2b519754e250
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816332"
---
# <a name="delete-partition"></a>Elimina partizione



Elimina la partizione con lo stato attivo.

## <a name="syntax"></a>Sintassi

```
delete partition [noerr] [override]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|eseguire l'override|Consente a DiskPart eliminare tutte le partizioni, indipendentemente dal tipo. In genere, DiskPart consente solo di eliminare le partizioni di dati noti.|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

> [!CAUTION]
> L'eliminazione di una partizione su un disco dinamico, è possibile eliminare tutti i volumi dinamici sul disco, pertanto la distruzione di tutti i dati e del disco in uno stato danneggiato. Per eliminare un volume dinamico, usare sempre la **eliminare il volume** invece di comando. Le partizioni possono essere eliminate dal dischi dinamici, ma non deve essere create. Ad esempio, è possibile eliminare una partizione di tabella di partizione GUID (GPT) non riconosciuta in un disco GPT dinamico. Eliminazione di una partizione non determina lo spazio liberato diventi disponibile. Questo comando è pensato per consentirti di spazio reclame su un disco dinamico offline danneggiato in una situazione di emergenza in cui il **pulita** Impossibile utilizzare il comando Diskpart.
-   È possibile eliminare la partizione di sistema, la partizione di avvio o qualsiasi partizione che contiene le active arresto anomalo o un file di dump informazioni di paging.
-   Per eseguire questa operazione, è necessario selezionare una partizione. Usare la **Seleziona partizione** comando per selezionare una partizione e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per eliminare la partizione con lo stato attivo, digitare:
```
delete partition
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

