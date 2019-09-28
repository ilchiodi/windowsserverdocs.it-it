---
title: bitsadmin create
description: 'Argomento dei comandi di Windows per **BITSAdmin create** : crea un processo di trasferimento con il nome visualizzato specificato.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9f6d641d44c56ea4ff11f48a725367de7dcf472a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381813"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un processo di trasferimento con il nome visualizzato specificato. Scaricare i processi trasferire i dati da un server a un file locale. I processi di caricamento trasferiscono i dati da un file locale a un server. I processi di caricamento-risposta trasferiscono i dati da un file locale a un server e ricevono un file di risposta dal server.

Usare l'opzione [BITSAdmin Resume](bitsadmin-resume.md) per attivare il processo nella coda di trasferimento.

## <a name="syntax"></a>Sintassi

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|type|-    **/download** trasferisce i dati da un server a un file locale.<br />-   **tra/upload** trasferisce i dati da un file locale a un server.<br />-    **/upload-Reply** trasferisce i dati da un file locale a un server e riceve un file di risposta dal server.<br />-Il valore predefinito di questo parametro è **/download** quando non è specificato nella riga di comando.|
|DisplayName|Nome visualizzato assegnato al processo appena creato.|

**BITS 1,2 e versioni precedenti**: I tipi tra/upload e/Upload-Reply non sono disponibili.

## <a name="BKMK_examples"></a>Esempi

Crea un processo di download denominato *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
