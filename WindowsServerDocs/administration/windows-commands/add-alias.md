---
title: add alias
description: Argomento di riferimento per il comando Aggiungi alias, che aggiunge alias all'ambiente alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 66301a39a1e969e270b42b5ce92a73392a134357
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819661"
---
# <a name="add-alias"></a>add alias

Aggiunge gli alias per l'ambiente di alias. Se utilizzata senza parametri, **aggiungere alias** Visualizza la Guida al prompt dei comandi. Gli alias vengono salvati nel file di metadati e verrà caricati con il **caricare i metadati** comando.

## <a name="syntax"></a>Sintassi

```
add alias <aliasname> <aliasvalue>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<aliasname>` | Specifica il nome dell'alias. |
| `<aliasvalue>` | Specifica il valore dell'alias. |
| `? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per elencare tutte le ombreggiature, inclusi i relativi alias, digitare:

```
list shadows all
```

L'estratto seguente mostra una copia shadow a cui è stato assegnato l'alias predefinito, *VSS_SHADOW_x*:

```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```

Per assegnare un nuovo alias con il nome *system1* a questa copia shadow, digitare:

```
add alias System1 %VSS_SHADOW_1%
```

In alternativa, è possibile assegnare l'alias utilizzando l'ID della copia shadow:

```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Load Metadata](load-metadata.md)
