---
title: bitsadmin addfile
description: Argomento dei comandi di Windows per **BITSAdmin AddFile** -aggiunge un file al processo specificato.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8dfddda92e506dbfca2a47394a310edf16fe78aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382038"
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
|RemoteURL|URL del file nel server.|
|LocalName|Nome del file nel computer locale. *LocalName* deve contenere un percorso assoluto del file.|

## <a name="BKMK_examples"></a>Esempi

Aggiungere un file al processo. Ripetere questa chiamata per ogni file che si desidera aggiungere. Se più processi usano *myDownloadJob* come nome, è necessario sostituire *MYDOWNLOADJOB* con il GUID del processo per identificare in modo univoco il processo.
```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)