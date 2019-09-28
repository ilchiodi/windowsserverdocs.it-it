---
title: bitsadmin gettype
description: 'Argomento dei comandi di Windows per **BITSAdmin GetType** : Recupera il tipo di processo del processo specificato.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca46cb813809621f4fa79b3265198206729a392c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381344"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype



Recupera il tipo di processo del processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetType <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Il tipo può essere DOWNLOAD, UPLOAD, UPLOAD-REPLY o UNKNOWN.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene recuperato il tipo di processo per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetType myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)