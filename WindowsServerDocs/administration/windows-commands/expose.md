---
title: esporre
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 51cc744bc2b61862ed05ca2e7d0aaa8f70d38692
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886662"
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
|\<Unità: >|Espone la copia shadow specificata come una lettera di unità (ad esempio, p).|
|\<Share>|Espone la copia shadow specificata in una condivisione (ad esempio, \\ \\ *MachineName*\).|
|\<MountPoint>|Espone la copia shadow specificata in un punto di montaggio (ad esempio, C:\shadowcopy\).|

## <a name="remarks"></a>Note

-   È possibile utilizzare un alias esistente o una variabile di ambiente al posto di *IDShadow*. Utilizzare **aggiungere** senza parametri per visualizzare gli alias esistenti.

## <a name="BKMK_examples"></a>Esempi

Per esporre la copia shadow permanente associata alla variabile di ambiente VSS_SHADOW_1 come unità X, digitare:
```
expose %vss_shadow_1% x:
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)