---
title: Caricare i metadati
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b52b5040fc8c834b04cad83ca4b0cfab103fdc43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871332"
---
# <a name="load-metadata"></a>Caricare i metadati



Carica un file con estensione cab metadati prima di importare una copia shadow trasportabili o carica i metadati del writer in caso di ripristino. Se utilizzata senza parametri, **caricare i metadati** Visualizza la Guida al prompt dei comandi.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<Drive>:][<Path>]|Specifica il percorso del file di metadati.|
|MetaData.cab|Specifica il file CAB di metadati da caricare.|

## <a name="remarks"></a>Note

-   È possibile utilizzare il **importare** comando per importare una copia shadow trasportabili in base ai metadati specificati da **caricare i metadati**.
-   Questo comando è necessario prima di **iniziare ripristino** comando per caricare il writer selezionato e i componenti per il ripristino.

## <a name="BKMK_examples"></a>Esempi

Per caricare un file di metadati denominato metafile.cab dal percorso predefinito, digitare:
```
load metadata metafile.cab
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)