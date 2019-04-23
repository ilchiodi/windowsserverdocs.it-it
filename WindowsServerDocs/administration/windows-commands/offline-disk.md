---
title: Disco non in linea
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 617371583a3f0cb3d0cb739845208e4216573d9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834622"
---
# <a name="offline-disk"></a>Disco non in linea



Accetta il disco online con lo stato attivo allo stato offline.

> [!IMPORTANT]
> Questo comando di DiskPart non è disponibile in qualsiasi edizione di Windows Vista.

## <a name="syntax"></a>Sintassi

```
offline disk [noerr]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Note

-   Questo comando viene eseguito nei dischi che sono in modalità online SAN. Modifica le modalità SAN non in linea.
-   Se un disco dinamico in un gruppo di dischi è offline, lo stato del disco diventa **mancante** e il gruppo è mostrato un disco non in linea. Disco mancante viene spostato al gruppo non valido. Se il disco dinamico è l'ultimo disco del gruppo, quindi lo stato del disco verrà modificato in **offline**, e verrà rimosso il gruppo vuoto.
-   È necessario selezionare un disco di **disco offline** comando abbia esito positivo. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="BKMK_examples"></a>Esempi

Per disconnettere il disco con lo stato attivo, digitare:
```
offline disk
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

