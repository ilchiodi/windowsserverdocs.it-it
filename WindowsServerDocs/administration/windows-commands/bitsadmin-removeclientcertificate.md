---
title: removeclientcertificate Bitsadmin
description: Argomento i comandi di Windows per **bitsadmin removeclientcertificate** -rimuove il certificato client dal processo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b720800fe80037f38ff01ac3a90d5cbb41a6ec8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868662"
---
# <a name="bitsadmin-removeclientcertificate"></a>removeclientcertificate Bitsadmin



Rimuove il certificato del client dal processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /RemoveClientCertificate <Job> 
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente rimuove il certificato del client dal processo denominato *myJob*.
```
C:\>Bitsadmin /RemoveClientCertificate myJob 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)