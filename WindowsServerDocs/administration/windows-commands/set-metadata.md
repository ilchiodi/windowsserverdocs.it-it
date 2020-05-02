---
title: Set di metadati
description: Argomento di riferimento per set Metadata, che consente di impostare il nome e il percorso del file di metadati per la creazione Shadow utilizzato per trasferire copie shadow da un computer a un altro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 683e54a7efc072d8709d6257771ba6bc5bde206e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721907"
---
# <a name="set-metadata"></a>Set di metadati

Imposta il nome e percorso del file di metadati creazione shadow utilizzato per trasferire le copie shadow da un computer a un altro. Se utilizzata senza parametri, **impostare i metadati** Visualizza la Guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[\<Unità>:] [<Path>]|Specifica il percorso per creare il file di metadati.|
|\<> metadati. cab|Specifica il nome del file cab per archiviare i metadati di creazione dell'ombreggiatura.|

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)