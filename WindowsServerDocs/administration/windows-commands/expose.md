---
title: esporre
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 819484364e8375c4d58e4d022681eedeaa7084ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377285"
---
# <a name="expose"></a>esporre



Espone una copia permanente come una lettera di unità, una condivisione o un punto di montaggio.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|IDShadow|Specifica l'ID di ombreggiatura della copia shadow che si desidera esporre.|
|\<Drive: >|Espone la copia shadow specificata come lettera di unità (ad esempio, P:).|
|\<Share >|Espone la copia shadow specificata in una condivisione, ad esempio \\ @ no__t-1*MachineName*\).|
|\<MountPoint >|Espone la copia shadow specificata a un punto di montaggio, ad esempio C:\shadowcopy @ no__t-0.|

## <a name="remarks"></a>Note

-   È possibile utilizzare un alias esistente o una variabile di ambiente al posto di *IDShadow*. Utilizzare **aggiungere** senza parametri per visualizzare gli alias esistenti.

## <a name="BKMK_examples"></a>Esempi

Per esporre la copia shadow persistente associata alla variabile di ambiente VSS_SHADOW_1 come unità X, digitare:
```
expose %vss_shadow_1% x:
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)