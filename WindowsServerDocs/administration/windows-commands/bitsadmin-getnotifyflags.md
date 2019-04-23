---
title: bitsadmin getnotifyflags
description: Argomento i comandi di Windows per **bitsadmin getnotifyflags** -recupera i flag di notifica per il processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 690e94805c5e61d96603e4ade102fb3a4bda409e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889282"
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

Il processo può contenere uno o più dei seguenti flag di notifica.

|---|---| | 0x001 | Generare un evento quando sono stati trasferiti tutti i file del processo. | | 0x002 | Generare un evento quando si verifica un errore. | | 0x004 | Disabilitare le notifiche. | | 0x008 | Generare un evento quando il processo viene modificato o viene effettuato lo stato del trasferimento. |

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera i flag di notifica per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)