---
title: Get-ImageFile
description: Argomento di riferimento per Get-ImageFile, che consente di recuperare informazioni sulle immagini contenute in un file di immagine Windows (wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f60be17f13e1436a0e895991c72d5ccb7130782
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719909"
---
# <a name="get-imagefile"></a>Get-ImageFile

Recupera le informazioni sulle immagini contenute in un file di immagine Windows (wim).

## <a name="syntax"></a>Sintassi

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/ImageFile:\<percorso del file WIM>|Specifica il percorso completo e il nome file del file con estensione wim.|
|[/ Dettagliate]|Restituisce tutti i metadati delle immagini da ogni immagine. Se si omette questa opzione, il comportamento predefinito è per restituire solo il nome dell'immagine, descrizione e nome file.|

## <a name="examples"></a>Esempi

Per visualizzare le informazioni su un'immagine, digitare:
```
WDSUTIL /Get-ImageFile /ImageFile:C:\temp\install.wim
```
Per visualizzare informazioni dettagliate, digitare:
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:\\Server\Share\My Folder \install.wim /Detailed
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)