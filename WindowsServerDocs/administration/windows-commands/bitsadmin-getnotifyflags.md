---
title: bitsadmin getnotifyflags
description: 'Argomento dei comandi di Windows per **BITSAdmin getnotifyflags** : recupera i flag di notifica per il processo specificato.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 56ee3a30050b6cc934b35bab24e9508911ea250e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381483"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags



Recupera i flag di notifica per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetNotifyFlags <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Il processo può contenere uno o più dei flag di notifica seguenti.

|-----|-----| | 0x001 | Genera un evento quando tutti i file del processo sono stati trasferiti. | | 0x002 | Genera un evento quando si verifica un errore. | | 0x004 | Disabilitare le notifiche. | | 0x008 | Genera un evento quando il processo viene modificato o viene effettuato lo stato del trasferimento. |

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente vengono recuperati i flag di notifica per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)