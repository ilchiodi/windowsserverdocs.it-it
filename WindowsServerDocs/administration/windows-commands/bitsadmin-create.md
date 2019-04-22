---
title: bitsadmin create
description: Argomento i comandi di Windows per **bitsadmin crea** -crea un processo di trasferimento con il nome visualizzato specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6ce5a4fdc21d879bf0a265e3c4185d83311464a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817192"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un processo di trasferimento con il nome visualizzato specificato. Download processi trasferimento dei dati da un server in un file locale. Caricare i dati di trasferimento dei processi da un file locale in un server. I processi di caricamento-risposta trasferire i dati da un file locale in un server e ricevano un file di risposta dal server.

Usare la [Riprendi bitsadmin](bitsadmin-resume.md) switch per attivare il processo nella coda di trasferimento.

## <a name="syntax"></a>Sintassi

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Tipo|-   **/ Scaricare** trasferisce i dati da un server in un file locale.<br />-   **/ Caricare** trasferisce i dati da un file locale in un server.<br />-   **/ Caricamento-risposta** trasferisce i dati da un file locale in un server e riceve un file di risposta dal server.<br />: Questo parametro viene impostato su **download** quando non è specificato nella riga di comando.|
|DisplayName|Il nome visualizzato assegnato al processo appena creato.|

**BITS 1.2 e versioni precedenti**: I tipi di /Upload-Reply e /upload. non sono disponibili.

## <a name="BKMK_examples"></a>Esempi

Crea un processo di download denominato *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
