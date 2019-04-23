---
title: bitsadmin setnotifyflags
description: Argomento i comandi di Windows per **bitsadmin setnotifyflags** -imposta l'evento di flag di notifica per il processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc817e03e0f1916ea392830d14985a7a1377d69a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868792"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Imposta l'evento di flag di notifica per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|NotifyFlags|Vedere la sezione Osservazioni|

## <a name="remarks"></a>Note

Il **NotifyFlags** parametro può contenere uno o più dei seguenti flag di notifica.

|---|---| | 1 | Generare un evento quando sono stati trasferiti tutti i file del processo. | | 2 | Generare un evento quando si verifica un errore. | | 4 | Disabilitare le notifiche. |

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta i flag di notifica per trasferiti e gli eventi di errore di processo per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)