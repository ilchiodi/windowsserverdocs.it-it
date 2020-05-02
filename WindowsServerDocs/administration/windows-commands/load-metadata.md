---
title: Caricare i metadati
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d3132bca86533ec3f2d0a27247bd3c116cf55b6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724459"
---
# <a name="load-metadata"></a>Caricare i metadati



Carica un file con estensione cab metadati prima di importare una copia shadow trasportabili o carica i metadati del writer in caso di ripristino. Se utilizzata senza parametri, **caricare i metadati** Visualizza la Guida al prompt dei comandi.



## <a name="syntax"></a>Sintassi

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<Unità>:] [<Path>]|Specifica il percorso del file di metadati.|
|MetaData.cab|Specifica il file CAB di metadati da caricare.|

## <a name="remarks"></a>Osservazioni

-   È possibile utilizzare il **importare** comando per importare una copia shadow trasportabili in base ai metadati specificati da **caricare i metadati**.
-   Questo comando è necessario prima di **iniziare ripristino** comando per caricare il writer selezionato e i componenti per il ripristino.

## <a name="examples"></a>Esempi

Per caricare un file di metadati denominato metafile.cab dal percorso predefinito, digitare:
```
load metadata metafile.cab
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)