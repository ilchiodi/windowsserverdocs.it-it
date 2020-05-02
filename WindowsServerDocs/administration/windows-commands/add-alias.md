---
title: Aggiungi alias
description: Argomento di riferimento per il comando Aggiungi alias, che aggiunge alias all'ambiente alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 807981c3581eea328291f2389e08065edbd280d3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719026"
---
# <a name="add-alias"></a>Aggiungi alias

Aggiunge gli alias per l'ambiente di alias. Se utilizzata senza parametri, **aggiungere alias** Visualizza la Guida al prompt dei comandi. Gli alias vengono salvati nel file di metadati e verrà caricati con il **caricare i metadati** comando.

## <a name="syntax"></a>Sintassi

```
add alias <AliasName> <AliasValue>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<AliasName>` | Specifica il nome dell'alias. |
| `<AliasValue>` | Specifica il valore dell'alias. |
| `/?` | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Load Metadata](load-metadata.md)
