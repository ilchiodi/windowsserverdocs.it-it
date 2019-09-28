---
title: "Bitsadmin getproxys: Recupera l'elenco di proxy per il processo specificato."
description: "Argomento dei comandi di Windows per **BITSAdmin getproxys** : Recupera l'elenco di proxy per il processo specificato."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f176d268c816725b183da0a948afcb25272b2fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381306"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera l'elenco di proxy per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /GetProxyList <Job>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|

## <a name="remarks"></a>Note

Il proxy è l'elenco dei server proxy da utilizzare. L'elenco è delimitato da virgole.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera l'elenco di proxy per il processo denominato *myDownloadJob*.
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)