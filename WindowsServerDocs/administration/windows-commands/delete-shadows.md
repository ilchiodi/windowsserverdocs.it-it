---
title: eliminare le ombreggiature
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b0d5414cb5b60face5245d7ea22d66c8629bfcb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873732"
---
# <a name="delete-shadows"></a>eliminare le ombreggiature



Consente di eliminare le copie shadow.

## <a name="syntax"></a>Sintassi

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|tutti|Elimina tutte le copie shadow.|
|volume \<Volume >|Elimina tutte le copie shadow del volume specificato.|
|meno recente \<Volume >|Elimina la copia shadow meno recente del volume specificato.|
|set \<SetID>|Elimina le copie shadow in un Set di copia Shadow dell'ID specificato. È possibile specificare un alias utilizzando il **%** simbolo, se l'alias è presente nell'ambiente corrente.|
|id \<ShadowID>|Elimina una copia dell'ID specificato. È possibile specificare un alias utilizzando il **%** simbolo, se l'alias è presente nell'ambiente corrente.|
|esposti {\<Drive > | <MountPoint>}|Elimina la copia shadow esposta nel punto di montaggio o lettera di unità specificata. Specificare i punti di montaggio come c:\mountPoint o la lettera di unità, ad esempio p.|

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)