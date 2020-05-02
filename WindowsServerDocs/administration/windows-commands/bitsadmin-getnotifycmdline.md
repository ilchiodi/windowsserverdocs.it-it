---
title: bitsadmin getnotifycmdline
description: Argomento di riferimento per il comando Bitsadmin getnotifycmdline, che consente di recuperare il comando della riga di comando che viene eseguito al termine del trasferimento dei dati da parte del processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90fa33e6-aca5-4a23-82bd-19a9f13f8416
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d4c47c4a1b9ea06fd804c8f2c48e9ac0ce1b319
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717793"
---
# <a name="bitsadmin-getnotifycmdline"></a>bitsadmin getnotifycmdline

Recupera il comando della riga di comando da eseguire al termine del trasferimento dei dati da parte del processo specificato.

> [!NOTE]
> Questo comando non è supportato da BITS 1,2 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getnotifycmdline <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare il comando della riga di comando utilizzato dal servizio quando il processo denominato *myDownloadJob* viene completato.

```
bitsadmin /getnotifycmdline myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
