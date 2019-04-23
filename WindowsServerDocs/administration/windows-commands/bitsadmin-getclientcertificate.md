---
title: getclientcertificate Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin getclientcertificate** -recupera il certificato client dal processo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 113d733d1deb9fbb1c89231495cb7af668a444d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869042"
---
# <a name="bitsadmin-getclientcertificate"></a>getclientcertificate Bitsadmin



Recupera il certificato client dal processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetClientCertificate <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera il certificato client per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin / GetClientCertificate myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)