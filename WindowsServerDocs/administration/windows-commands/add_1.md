---
title: aggiungi
description: "Argomento i comandi di Windows per **add_1** : aggiunge i volumi per il set di volumi che devono essere replicati mediante copiata shadow o aggiunge gli alias per l'ambiente di alias."
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 549c8560774f004a60926ce568c850fd1b71c7f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889952"
---
# <a name="add"></a>aggiungi


Aggiunge volumi per il set di volumi che devono essere replicati mediante copiata shadow, o gli alias per l'ambiente di alias. Se utilizzata senza, sottocomandi **aggiungere** sono elencati i volumi correnti e gli alias.

> [!NOTE]
> Gli alias non vengono aggiunte all'ambiente di alias fino a quando non viene creata la copia shadow. Gli alias che è necessario immediatamente devono essere aggiunti utilizzando **aggiungere alias**.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
add 
add volume <Volume> [provider <ProviderID>] 
add alias <AliasName> <AliasValue>
```

## <a name="add-subcommands"></a>Aggiungere sottocomandi

|Sottocomando|Descrizione|
|----------|-----------|
|volume|Aggiunge un volume al Set di copia Shadow, ovvero il set di volumi per la copia shadow. Visualizzare [Aggiungi volume](add-volume.md) per la sintassi e parametri.|
|alias|Aggiunge il nome specificato e il valore per l'ambiente di alias. Visualizzare [aggiungere un alias](add-alias.md) per la sintassi e parametri.|
|/?|Visualizza la Guida dalla riga di comando.|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare i volumi aggiunti e gli alias che sono attualmente presenti nell'ambiente, digitare:
```
add
```
L'output seguente mostra che l'unità C è stato aggiunto al Set di copia Shadow:
```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)