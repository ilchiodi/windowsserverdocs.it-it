---
title: bitsadmin wrap
description: Argomento i comandi di Windows per **incapsulamento bitsadmin** -esegue il wrapping di tutte le righe di output testo che si estende oltre il bordo all'estrema destra della finestra di comando nella riga successiva.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4834a8a17c72394b6ee8f051ec76919af9880124
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881672"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il wrapping di output regolare in una finestra di comando.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Wrap Job
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Specificare prima di altre opzioni. Per impostazione predefinita, tutte le opzioni, ad eccezione di [monitoraggio bitsadmin](bitsadmin-monitor.md) commutatore, eseguire il wrapping dell'output.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera le informazioni per il processo denominato *myDownloadJob* e include l'output.

```
C:\>bitsadmin /Wrap /Info myDownloadJob /verbose
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
