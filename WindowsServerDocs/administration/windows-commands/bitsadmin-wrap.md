---
title: bitsadmin wrap
description: Argomento comandi di Windows per **BITSAdmin wrap** -wrapping di qualsiasi riga di testo di output che si estende oltre il bordo all'estrema destra della finestra di comando alla riga successiva.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5609fb6f38716795a545e0c7fe3939f893a8c8d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380685"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il wrapping dell'output per adattarlo a una finestra di comando.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Specificare prima di altre opzioni. Per impostazione predefinita, tutte le opzioni, ad eccezione dell'opzione di [monitoraggio Bitsadmin](bitsadmin-monitor.md) , incapsulano l'output.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera le informazioni per il processo denominato *myDownloadJob* e include l'output.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
