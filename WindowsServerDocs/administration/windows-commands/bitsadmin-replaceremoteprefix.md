---
title: bitsadmin replaceremoteprefix
description: Argomento i comandi di Windows per **bitsadmin replaceremoteprefix** -tutti i file nel processo il cui URL remoto inizia con *OldPrefix* sono state modificate per utilizzare *NewPrefix*.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25c0f997ea0b9f97051baa291bdf87c84b6b1cbb
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811294"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

Tutti i file del processo il cui URL remoto inizia con *OldPrefix* vengono modificate per l'utilizzo di *NewPrefix*.

## <a name="syntax"></a>Sintassi

```
bitsadmin /ReplaceRemotePrefix <Job> <OldPrefix> <NewPrefix
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|OldPrefix|Prefisso URL esistente|
|NewPrefix|Nuovo prefisso URL|

## <a name="examples"></a>Esempi

L'esempio seguente modifica tutti i file nel processo denominato *myDownloadJob* cui URL remoto inizia con *http://stageserver* alla *http://prodserver* .

```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Altre informazioni

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)