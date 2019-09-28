---
title: disco attributi
description: Windows Commands argomento for **Attributes disk** -Visualizza, imposta o cancella gli attributi di un disco.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 415125208b13d82adeed736107f59fda9489a953
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382573"
---
# <a name="attributes-disk"></a>disco attributi



Visualizza, imposta o cancella gli attributi di un disco.

> [!IMPORTANT]
> Questo parametro non è disponibile nelle edizioni di Windows Vista.

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
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Quando viene utilizzato il **disco attributi** per visualizzare gli attributi correnti di un disco, l'attributo del disco di avvio indica il disco utilizzato per avviare il computer. Per un mirroring dinamico, viene visualizzato per il disco che contiene il plex di avvio del volume di avvio.
-   Per avere esito positivo, è necessario selezionare un disco per il comando del **disco attributi** . Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per visualizzare gli attributi del disco selezionato, digitare:
```
attributes disk
```
Per impostare il disco selezionato come di sola lettura, digitare:
```
attributes disk set readonly
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

