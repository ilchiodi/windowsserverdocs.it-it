---
title: bitsadmin setnoprogresstimeout
description: Windows Commands Topic for Bitsadmin setnoprogresstimeout, che consente di impostare il periodo di tempo, in secondi, durante il quale il servizio tenta di trasferire il file dopo che si è verificato un errore temporaneo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 544a6c73f29684bc4091ec05fa28016fbc718bb2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849354"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Imposta il periodo di tempo, in secondi, durante il quale BITS tenta di trasferire il file dopo il primo errore temporaneo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|TimeOutvalue|Un numero rappresentato in secondi.|

## <a name="remarks"></a>Note

-   L'intervallo di timeout non lo stato di avanzamento inizia quando il processo si verifica un errore temporaneo.
-   L'intervallo di timeout si arresta o viene reimpostato quando viene trasferito un byte di dati.
-   Se l'intervallo di timeout non lo stato di avanzamento supera il *TimeOutvalue*, quindi il processo viene inserito in uno stato di errore irreversibile.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente non imposta alcun valore di timeout lo stato di avanzamento del processo *myDownloadJob* su 20 secondi
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)