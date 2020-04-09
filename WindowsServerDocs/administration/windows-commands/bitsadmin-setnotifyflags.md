---
title: bitsadmin setnotifyflags
description: Windows Commands Topic for Bitsadmin setnotifyflags, che imposta i flag di notifica degli eventi per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd3001fa4ae7f51cab92556f4f2f498511cca5ae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849284"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Imposta l'evento di flag di notifica per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|NotifyFlags|Vedere la sezione Osservazioni|

## <a name="remarks"></a>Note

Il parametro **NotifyFlags** può contenere uno o più dei flag di notifica seguenti.

|-----|-----| | 1 | Genera un evento quando tutti i file del processo sono stati trasferiti. | | 2 | Genera un evento quando si verifica un errore. | | 4 | Disabilitare le notifiche. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente imposta i flag di notifica per trasferiti e gli eventi di errore di processo per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)