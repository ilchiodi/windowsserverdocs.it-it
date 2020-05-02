---
title: esporre
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 500dc5cfcd5e2bba4cfbc3cb5ef81a9065ea53cf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725673"
---
# <a name="expose"></a>esporre



Espone una copia permanente come una lettera di unità, una condivisione o un punto di montaggio.



## <a name="syntax"></a>Sintassi

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|IDShadow|Specifica l'ID di ombreggiatura della copia shadow che si desidera esporre.|
|\<Unità: >|Espone la copia shadow specificata come lettera di unità (ad esempio, P:).|
|\<Condividi>|Espone la copia shadow specificata in una condivisione, \\ \\ad esempio *machineName*\).|
|\<> MountPoint|Espone la copia shadow specificata a un punto di montaggio, ad esempio C:\shadowcopy\).|

## <a name="remarks"></a>Osservazioni

-   È possibile utilizzare un alias esistente o una variabile di ambiente al posto di *IDShadow*. Utilizzare **aggiungere** senza parametri per visualizzare gli alias esistenti.

## <a name="examples"></a>Esempi

Per esporre la copia shadow persistente associata alla variabile di ambiente VSS_SHADOW_1 come unità X, digitare:
```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)