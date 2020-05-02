---
title: Elimina partizione
description: Argomento di riferimento per Delete Partition, che elimina la partizione con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 259abdfc6e3ba8db22d5582bff08d7a4bc8b807b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716723"
---
# <a name="delete-partition"></a>Elimina partizione

Elimina la partizione con lo stato attivo.

## <a name="syntax"></a>Sintassi

```
delete partition [noerr] [override]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|override|Consente a DiskPart di eliminare qualsiasi partizione indipendentemente dal tipo. In genere, DiskPart consente di eliminare solo le partizioni di dati note.|
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Osservazioni

> [!CAUTION]
> L'eliminazione di una partizione in un disco dinamico consente di eliminare tutti i volumi dinamici sul disco, eliminando così tutti i dati e lasciando il disco in uno stato danneggiato. Per eliminare un volume dinamico, utilizzare sempre il comando **delete volume** . Le partizioni possono essere eliminate dai dischi dinamici, ma non devono essere create. È ad esempio possibile eliminare una partizione GPT (GUID Partition Table) non riconosciuta in un disco GPT dinamico. L'eliminazione di una partizione di questo tipo non comporta la disponibilità dello spazio libero risultante. Questo comando consente di liberare spazio su un disco dinamico offline danneggiato in una situazione di emergenza in cui non è possibile usare il comando **Pulisci** in Diskpart.
> -   Non è possibile eliminare la partizione di sistema, la partizione di avvio o una partizione che contiene il file di paging attivo o le informazioni di dump di arresto anomalo.
> -   Per eseguire questa operazione, è necessario selezionare una partizione. Usare il comando **select partition** per selezionare una partizione e spostare lo stato attivo su di essa.

## <a name="examples"></a>Esempi

Per eliminare la partizione con lo stato attivo, digitare:
```
delete partition
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

