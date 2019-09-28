---
title: bitsadmin getproxyusage
description: "Windows Commands Topic for **BITSAdmin getproxyusage** : Recupera l'impostazione di utilizzo del proxy per il processo specificato."
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ea9a22f4fb35af3436d02d9f23b62ce0888a26b0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381293"
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

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)