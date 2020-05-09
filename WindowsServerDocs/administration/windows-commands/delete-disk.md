---
title: Elimina disco
description: Argomento di riferimento per il comando delete disk, che elimina un disco dinamico mancante dall'elenco dei dischi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c8076f486251e428bce8805e15c2aa74caaf834
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993136"
---
# <a name="delete-disk"></a>Elimina disco

Elimina un disco dinamico mancante dall'elenco dei dischi.

> [!NOTE]
> Per istruzioni dettagliate sull'uso di questo comando, vedere [rimuovere un disco dinamico mancante](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753029(v=ws.11)).

## <a name="syntax"></a>Sintassi

```
delete disk [noerr] [override]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| NOERR | Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore. |
| override | Consente a DiskPart di eliminare tutti i volumi semplici sul disco. Se il disco contiene metà di un volume con mirroring, la metà del mirror sul disco viene eliminata. Il comando Elimina override disco ha esito negativo se il disco è membro di un volume RAID-5. |

## <a name="examples"></a>Esempi

Per eliminare un disco dinamico mancante dall'elenco dei dischi, digitare:

```
delete disk
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Delete](delete.md)
