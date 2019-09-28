---
title: bitsadmin geterrorcount
description: 'Argomento dei comandi di Windows per **BITSAdmin GetErrorCount** : Recupera un conteggio del numero di volte in cui il processo specificato ha generato un errore temporaneo.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e5aa64c0e080e946e84c0bf804527bb00cad70a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381625"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount



Recupera un conteggio del numero di volte in cui il processo specificato ha generato un errore temporaneo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetErrorCount <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente vengono recuperate le informazioni sul conteggio degli errori per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetErrorCount myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)