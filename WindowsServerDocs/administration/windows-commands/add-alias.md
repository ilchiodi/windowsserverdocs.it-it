---
title: Aggiungi alias
description: "Argomento comandi di Windows per **Aggiungi alias** : aggiunge alias all'ambiente alias."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2834376e497f54eadf1d9077e74f9c398202c5a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382822"
---
# <a name="add-alias"></a>Aggiungi alias



Aggiunge gli alias per l'ambiente di alias. Se utilizzata senza parametri, **aggiungere alias** Visualizza la Guida al prompt dei comandi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
add alias <AliasName> <AliasValue>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<AliasName >|Specifica il nome dell'alias.|
|\<AliasValue >|Specifica il valore dell'alias.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Gli alias vengono salvati nel file di metadati e verrà caricati con il **caricare i metadati** comando.

## <a name="BKMK_examples"></a>Esempi

Per elencare tutte le ombreggiature, inclusi i relativi alias, digitare:
```
list shadows all
```
L'estratto seguente mostra una copia shadow a cui è stato assegnato l'alias predefinito, VSS_SHADOW_x:
```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```
Per assegnare un nuovo alias con il nome System1 la copia shadow, digitare:
```
add alias System1 %VSS_SHADOW_1%
```
In alternativa, è possibile assegnare l'alias utilizzando l'ID della copia shadow:
```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)