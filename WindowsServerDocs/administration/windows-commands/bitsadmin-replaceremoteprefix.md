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
ms.openlocfilehash: 3d3eba4f62842fa7f862cd4eaea6830e6a08397a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868132"
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

## <a name="BKMK_examples"></a>Esempi

L'esempio seguente modifica tutti i file nel processo denominato *myDownloadJob* cui URL remoto inizia con *http://stageserver* alla *http://prodserver*.
```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Altre informazioni

[Chiave sintassi della riga di comando](command-line-syntax-key.md)