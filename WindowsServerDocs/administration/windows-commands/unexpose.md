---
title: volume x:.
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e10126739ef82b060e271e9b804a77658b5ec82
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392270"
---
# <a name="unexpose"></a>volume x:.



Unexposes una copia shadow esposta tramite il **esporre** comando. La copia shadow esposta può essere specificata dal relativo ID Shadow, lettera di unità, una condivisione o un punto di montaggio.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<IDShadow >|Unexposes la copia shadow specificata per l'ID di Shadow specificato.|
|Unità \<: >|Non espone la copia shadow associata alla lettera di unità specificata (ad esempio, l'unità P).|
|Condivisione \<>|Non espone la copia shadow associata alla condivisione specificata, ad esempio \\\\*MachineName*\).|
|\<MountPoint >|Non espone la copia shadow associata al punto di montaggio specificato, ad esempio C:\shadowcopy\).|

## <a name="remarks"></a>Osservazioni

-   È possibile utilizzare un alias esistente o una variabile di ambiente al posto di *IDShadow*. Utilizzare **aggiungere** senza parametri per visualizzare gli alias esistenti.

## <a name="BKMK_examples"></a>Esempi

Per annullare l'esposizione della copia shadow associata P unità, digitare:
```
unexpose P:
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)