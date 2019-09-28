---
title: bitsadmin replaceremoteprefix
description: Argomento dei comandi di Windows per **BITSAdmin REPLACEREMOTEPREFIX** -tutti i file nel processo il cui URL remoto inizia con *OldPrefix* vengono modificati in modo da usare *NewPrefix*.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ee896a337b571487797967d3ce0bf1f1b17e7507
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380804"
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

Nell'esempio seguente vengono modificati tutti i file nel processo denominato *myDownloadJob* il cui URL remoto inizia con *http://stageserver* *http://prodserver* .

```
C:\>bitsadmin /ReplaceRemotePrefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Altre informazioni

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)