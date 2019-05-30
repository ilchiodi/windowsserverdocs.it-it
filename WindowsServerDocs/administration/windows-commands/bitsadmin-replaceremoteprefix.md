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
ms.openlocfilehash: 848c57736c3530e296cffb970237149b4634de67
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266518"
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