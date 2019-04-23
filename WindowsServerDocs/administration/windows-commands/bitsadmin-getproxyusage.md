---
title: bitsadmin getproxyusage
description: Argomento i comandi di Windows per **bitsadmin getproxyusage** -recupera l'impostazione di utilizzo di proxy per il processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20ba418b8dfcf3d96d9b20b22e53797a232a13f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863882"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage



Recupera l'impostazione di utilizzo di proxy per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetProxyUsage <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

I valori possibili sono:
-   PRECONFIGURAZIONE, utilizzare i valori predefiniti di Internet Explorer del proprietario.
-   NO_PROXY: non utilizzare un server proxy.
-   Eseguire l'OVERRIDE, utilizzare un elenco di proxy esplicito.
-   Rilevamento AUTOMATICO, rilevare automaticamente le impostazioni del proxy.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente viene recuperato l'utilizzo di proxy per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetProxyUsage myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)