---
title: bitsadmin setnoprogresstimeout
description: 'Argomento dei comandi di Windows per **BITSAdmin setnoprogresstimeout** : consente di impostare il periodo di tempo, in secondi, durante il quale il servizio tenta di trasferire il file dopo che si è verificato un errore temporaneo.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 761d0d76a2c70af9d4ad68aa564c1a9816691d0d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380503"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Imposta il periodo di tempo, in secondi, durante il quale BITS tenta di trasferire il file dopo il primo errore temporaneo.

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

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)