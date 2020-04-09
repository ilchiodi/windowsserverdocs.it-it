---
title: bitsadmin getnotifyflags
description: Windows Commands Topic for **BITSAdmin getnotifyflags**, che recupera i flag di notifica per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3138baea05f793cfb587d3f8fb669d446daea6b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850584"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags

Recupera i flag di notifica per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getnotifyflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="remarks"></a>Note

Il processo può contenere uno o più dei flag di notifica seguenti:

| Flag | Descrizione |
| ----- | ----- |
| 0x001 | Generare un evento quando sono stati trasferiti tutti i file del processo. |
| 0x002 | Generare un evento quando si verifica un errore. |
| 0x004 | Disabilitare le notifiche. |
| 0x008 | Genera un evento quando il processo viene modificato o viene eseguito lo stato del trasferimento. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono recuperati i flag di notifica per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getnotifyflags myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)