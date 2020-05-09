---
title: Elimina partizione
description: Argomento di riferimento per il comando Delete Partition, che consente di eliminare la partizione con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13c79b826480171af578334942af8f73b77d796b
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993120"
---
# <a name="delete-partition"></a>Elimina partizione

Elimina la partizione con lo stato attivo. Prima di iniziare, è necessario selezionare una partizione affinché questa operazione abbia esito positivo. Usare il comando [select partition](select-partition.md) per selezionare una partizione e spostare lo stato attivo su di essa.

> [!WARNING]
> L'eliminazione di una partizione in un disco dinamico consente di eliminare tutti i volumi dinamici sul disco, eliminando tutti i dati e lasciando il disco in uno stato danneggiato.
>
> Non è possibile eliminare la partizione di sistema, la partizione di avvio o qualsiasi partizione che contiene il file di paging attivo o le informazioni di dump di arresto anomalo.

## <a name="syntax"></a>Sintassi

```
delete partition [noerr] [override]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |
| override | Consente a DiskPart di eliminare qualsiasi partizione indipendentemente dal tipo. In genere, DiskPart consente di eliminare solo le partizioni di dati note. |

#### <a name="remarks"></a>Osservazioni

- Per eliminare un volume dinamico, utilizzare sempre il comando [delete volume](delete-volume.md) .

- Le partizioni possono essere eliminate dai dischi dinamici, ma non devono essere create. È ad esempio possibile eliminare una partizione GPT (GUID Partition Table) non riconosciuta in un disco GPT dinamico. Se si elimina una partizione di questo tipo, lo spazio libero risultante diventerà disponibile. Al contrario, questo comando consente di recuperare spazio su un disco dinamico offline danneggiato in una situazione di emergenza in cui non è possibile usare il comando [Pulisci](clean.md) in Diskpart.

## <a name="examples"></a>Esempi

Per eliminare la partizione con lo stato attivo, digitare:

```
delete partition
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Seleziona partizione](select-partition.md)

- [comando Delete](delete.md)

- [comando Elimina volume](delete-volume.md)

- [Pulisci comando](clean.md)
