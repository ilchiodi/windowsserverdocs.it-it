---
title: disco offline
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a00b7f51bc950c3737ba6350fe15a7a4c6cc45e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723440"
---
# <a name="offline-disk"></a>disco offline



Accetta il disco online con lo stato attivo allo stato offline.

> [!IMPORTANT]
> Questo comando di DiskPart non è disponibile in qualsiasi edizione di Windows Vista.

## <a name="syntax"></a>Sintassi

```
offline disk [noerr]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|NOERR|Solo per gli script. Quando si è verificato un errore, DiskPart continua a elaborare i comandi come se non si è verificato l'errore. Senza questo parametro, un errore causa DiskPart viene interrotto con un codice di errore.|

## <a name="remarks"></a>Osservazioni

-   Questo comando viene eseguito nei dischi che sono in modalità online SAN. Modifica le modalità SAN non in linea.
-   Se un disco dinamico in un gruppo di dischi è offline, lo stato del disco diventa **mancante** e il gruppo è mostrato un disco non in linea. Disco mancante viene spostato al gruppo non valido. Se il disco dinamico è l'ultimo disco del gruppo, quindi lo stato del disco verrà modificato in **offline**, e verrà rimosso il gruppo vuoto.
-   È necessario selezionare un disco di **disco offline** comando abbia esito positivo. Utilizzare il **disco selezionare** comando per selezionare un disco e spostare lo stato attivo a esso.

## <a name="examples"></a>Esempi

Per disconnettere il disco con lo stato attivo, digitare:
```
offline disk
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

