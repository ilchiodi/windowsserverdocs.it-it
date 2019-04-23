---
title: bitsadmin addfile
description: Argomento i comandi di Windows per **bitsadmin addfile** -aggiunge un file al processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c3027bdc4f3f8f3e3ca50400b2c5dbf33bf2bc5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861752"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Aggiunge un file al processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|RemoteURL|L'URL del file nel server.|
|LocalName|Il nome del file nel computer locale. *LocalName* deve contenere un percorso assoluto del file.|

## <a name="BKMK_examples"></a>Esempi

Aggiungere un file al processo. Ripetere questa chiamata per ogni file da aggiungere. Se più processi utilizzano *myDownloadJob* per il proprio nome, è necessario sostituire *myDownloadJob* con il GUID del processo per identificare in modo univoco il processo.
```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)