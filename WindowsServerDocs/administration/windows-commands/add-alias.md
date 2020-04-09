---
title: Aggiungi alias
description: Windows Commands argomento per **Add alias**, che aggiunge alias all'ambiente alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebffc1504f502711dab30f6f9b120ad20e64ae9d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851364"
---
# <a name="add-alias"></a>Aggiungi alias

Aggiunge gli alias per l'ambiente di alias. Se utilizzata senza parametri, **aggiungere alias** Visualizza la Guida al prompt dei comandi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
add alias <AliasName> <AliasValue>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|`<AliasName>`|Specifica il nome dell'alias.|
|`<AliasValue>`|Specifica il valore dell'alias.|
|`/?`|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Gli alias vengono salvati nel file di metadati e verrà caricati con il **caricare i metadati** comando.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)