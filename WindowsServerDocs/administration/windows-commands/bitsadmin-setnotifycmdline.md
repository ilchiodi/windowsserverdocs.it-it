---
title: bitsadmin setnotifycmdline
description: Windows Commands Topic for **BITSAdmin setnotifycmdline**, che consente di impostare il comando della riga di comando che viene eseguito al termine del processo di trasferimento dei dati o quando un processo entra in uno stato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b268b68cbd355a7fe7f993d678a98f6fcb99f0ab
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122891"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Imposta il comando della riga di comando che viene eseguito al termine del processo di trasferimento dei dati o quando un processo entra in uno stato specificato.

> [!NOTE]
> Questo comando non è supportato da BITS 1,2 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setnotifycmdline <job> <program_name> [program_parameters]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |
| program_name | Nome del comando da eseguire al termine del processo. È possibile impostare questo valore come NULL, ma in tal caso, *program_parameters* necessario impostare anche su null. |
| program_parameters | Parametri che si desidera passare al *program_name*. È possibile impostare questo valore come NULL. Se *program_parameters* non è impostato su null, il primo parametro in *program_parameters* deve corrispondere al *program_name*. |

## <a name="examples"></a>Esempi

Nell'esempio seguente viene impostato il comando della riga di comando utilizzato dal servizio per eseguire notepad. exe al termine del processo denominato *myDownloadJob* .

```
C:\>bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe NULL
```

```
C:\>bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)