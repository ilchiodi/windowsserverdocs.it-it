---
title: Elimina partizione
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 46a214f26e7c21f6ae08eb16d95fd898bd949b0f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378660"
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
|eseguire l'override|Consente a DiskPart di eliminare qualsiasi partizione indipendentemente dal tipo. In genere, DiskPart consente di eliminare solo le partizioni di dati note.|
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

> [!CAUTION]
> L'eliminazione di una partizione in un disco dinamico consente di eliminare tutti i volumi dinamici sul disco, eliminando così tutti i dati e lasciando il disco in uno stato danneggiato. Per eliminare un volume dinamico, utilizzare sempre il comando **delete volume** . Le partizioni possono essere eliminate dai dischi dinamici, ma non devono essere create. È ad esempio possibile eliminare una partizione GPT (GUID Partition Table) non riconosciuta in un disco GPT dinamico. L'eliminazione di una partizione di questo tipo non comporta la disponibilità dello spazio libero risultante. Questo comando consente di liberare spazio su un disco dinamico offline danneggiato in una situazione di emergenza in cui non è possibile usare il comando **Pulisci** in Diskpart.
> -   Non è possibile eliminare la partizione di sistema, la partizione di avvio o una partizione che contiene il file di paging attivo o le informazioni di dump di arresto anomalo.
> -   Per eseguire questa operazione, è necessario selezionare una partizione. Usare il comando **select partition** per selezionare una partizione e spostare lo stato attivo su di essa.

## <a name="BKMK_examples"></a>Esempi

Per eliminare la partizione con lo stato attivo, digitare:
```
delete partition
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

