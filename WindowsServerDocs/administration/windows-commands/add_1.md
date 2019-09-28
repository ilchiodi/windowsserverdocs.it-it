---
title: aggiungi
description: "Argomento dei comandi di Windows per **add_1** : aggiunge i volumi al set di volumi che devono essere replicati o aggiunge alias all'ambiente alias."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1aaa211938d14a0019d29e64867f4df2475a877
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382778"
---
# <a name="add"></a>aggiungi


Aggiunge volumi al set di volumi di cui deve essere eseguita la copia shadow o aggiunge alias all'ambiente alias. Se usato senza sottocomandi, **Aggiungi** elenca i volumi e gli alias correnti.

> [!NOTE]
> Gli alias non vengono aggiunti all'ambiente alias fino a quando non viene creata la copia shadow. Gli alias che è necessario aggiungere immediatamente devono essere aggiunti usando **Aggiungi alias**.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
add 
add volume <Volume> [provider <ProviderID>] 
add alias <AliasName> <AliasValue>
```

## <a name="add-subcommands"></a>Aggiungi sottocomandi

|Sottocomando|Descrizione|
|----------|-----------|
|volume|Aggiunge un volume al set di copie shadow, ovvero il set di volumi da replicare. Vedere [aggiungere un volume](add-volume.md) per la sintassi e i parametri.|
|alias|Aggiunge il nome e il valore specificati all'ambiente alias. Vedere [aggiungere alias](add-alias.md) per sintassi e parametri.|
|/?|Visualizza la guida nella riga di comando.|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare i volumi aggiunti e gli alias attualmente presenti nell'ambiente, digitare:
```
add
```
L'output seguente mostra che l'unità C è stata aggiunta al set di copie shadow:
```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)