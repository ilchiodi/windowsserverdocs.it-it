---
title: disco di attributi
description: Argomento i comandi di Windows per **disco attributi** -Visualizza, imposta o Cancella gli attributi di un disco.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cacc2fb6b47d095f5e452ca470c89f228949594
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890352"
---
# <a name="attributes-disk"></a>disco di attributi



Visualizza, imposta o Cancella gli attributi di un disco.

> [!IMPORTANT]
> Questo parametro non è disponibile in alcuna edizione di Windows Vista.

## <a name="syntax"></a>Sintassi

```
attributes disk [{set | clear}] [readonly] [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|set|Imposta l'attributo specificato del disco con lo stato attivo.|
|clear|Cancella l'attributo specificato del disco con lo stato attivo.|
|readonly|Specifica che il disco è di sola lettura.|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Quando **disco attributi** viene usato per visualizzare gli attributi correnti di un disco, l'attributo di disco di avvio denota il disco che viene usato per avviare il computer. Per un mirroring dinamico, viene visualizzato per il disco che contiene il plessi di avvio del volume di avvio.
-   È necessario selezionare un disco per il **disco attributi** comando abbia esito positivo. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare gli attributi del disco selezionato, digitare:
```
attributes disk
```
Per impostare il disco selezionato in sola lettura, digitare:
```
attributes disk set readonly
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

