---
title: bitsadmin setnotifyflags
description: Argomento dei comandi di Windows per **BITSAdmin setnotifyflags** -imposta i flag di notifica degli eventi per il processo specificato.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d9cfabf05610cbbe8fa65fd16b0d33e161dcef9b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380447"
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

Il parametro **NotifyFlags** può contenere uno o più dei flag di notifica seguenti.

|-----|-----| | 1 | Genera un evento quando tutti i file del processo sono stati trasferiti. | | 2 | Genera un evento quando si verifica un errore. | | 4 | Disabilitare le notifiche. |

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente imposta i flag di notifica per trasferiti e gli eventi di errore di processo per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)