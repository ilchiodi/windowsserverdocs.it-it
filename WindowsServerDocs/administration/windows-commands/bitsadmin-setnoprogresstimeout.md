---
title: bitsadmin setnoprogresstimeout
description: Argomento i comandi di Windows per **bitsadmin setnoprogresstimeout** -imposta il periodo di tempo, espresso in secondi, che il servizio tenterà di trasferire il file dopo che si verifica un errore temporaneo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45dd8a7ddfae877984a98db66c742e0af4d18f0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873772"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Imposta il periodo di tempo, espresso in secondi, che BITS tenta di trasferire il file dopo il primo errore temporaneo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|TimeOutvalue|Un numero rappresentato in secondi.|

## <a name="remarks"></a>Note

-   L'intervallo di timeout non lo stato di avanzamento inizia quando il processo si verifica un errore temporaneo.
-   L'intervallo di timeout si arresta o viene reimpostato quando viene trasferito un byte di dati.
-   Se l'intervallo di timeout non lo stato di avanzamento supera il *TimeOutvalue*, quindi il processo viene inserito in uno stato di errore irreversibile.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente non imposta alcun valore di timeout lo stato di avanzamento del processo *myDownloadJob* su 20 secondi
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)