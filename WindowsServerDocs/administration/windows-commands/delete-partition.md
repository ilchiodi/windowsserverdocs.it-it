---
title: Elimina partizione
description: Windows Commands argomento for Delete Partition, che elimina la partizione con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a24c18cf98f2899fbb57f1f5f2d2776824d637b7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846574"
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
|NOERR|solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

> [!CAUTION]
> L'eliminazione di una partizione in un disco dinamico consente di eliminare tutti i volumi dinamici sul disco, eliminando così tutti i dati e lasciando il disco in uno stato danneggiato. Per eliminare un volume dinamico, utilizzare sempre il comando **delete volume** . Le partizioni possono essere eliminate dai dischi dinamici, ma non devono essere create. È ad esempio possibile eliminare una partizione GPT (GUID Partition Table) non riconosciuta in un disco GPT dinamico. L'eliminazione di una partizione di questo tipo non comporta la disponibilità dello spazio libero risultante. Questo comando consente di liberare spazio su un disco dinamico offline danneggiato in una situazione di emergenza in cui non è possibile usare il comando **Pulisci** in Diskpart.
> -   Non è possibile eliminare la partizione di sistema, la partizione di avvio o una partizione che contiene il file di paging attivo o le informazioni di dump di arresto anomalo.
> -   Per eseguire questa operazione, è necessario selezionare una partizione. Usare il comando **select partition** per selezionare una partizione e spostare lo stato attivo su di essa.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per eliminare la partizione con lo stato attivo, digitare:
```
delete partition
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

