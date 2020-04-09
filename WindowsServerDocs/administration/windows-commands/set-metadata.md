---
title: Set di metadati
description: Windows Commands Topic for set Metadata, che imposta il nome e il percorso del file di metadati per la creazione Shadow utilizzato per trasferire copie shadow da un computer a un altro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5bd728650cf163f98a82ff1e6f88755c4cc1aea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834524"
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
|[\<unità >:] [<Path>]|Specifica il percorso per creare il file di metadati.|
|\<metadati. cab >|Specifica il nome del file cab per archiviare i metadati di creazione dell'ombreggiatura.|

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)