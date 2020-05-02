---
title: bitsadmin setnotifycmdline
description: Argomento di riferimento per il comando Bitsadmin setnotifycmdline, che consente di impostare il comando della riga di comando che viene eseguito al termine del trasferimento dei dati da parte del processo o quando un processo entra in uno stato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b21d7151a5b646a4fe07d073220614f5e3c99539
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720126"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Imposta il comando della riga di comando che viene eseguito al termine del processo di trasferimento dei dati o dopo l'immissione di un processo in uno stato specificato.

> [!NOTE]
> Questo comando non è supportato da BITS 1,2 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setnotifycmdline <job> <program_name> [program_parameters]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| program_name | Nome del comando da eseguire al termine del processo. È possibile impostare questo valore come NULL, ma in tal caso, *program_parameters* necessario impostare anche su null. |
| program_parameters | Parametri che si desidera passare al *program_name*. È possibile impostare questo valore come NULL. Se *program_parameters* non è impostato su null, il primo parametro in *program_parameters* deve corrispondere al *program_name*. |

## <a name="examples"></a>Esempi

Per eseguire notepad. exe al termine del processo denominato *myDownloadJob*:

```
bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe NULL
```

Per visualizzare il testo EULA in Notepad. exe, al termine del processo denominato myDownloadJob:

```
bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
