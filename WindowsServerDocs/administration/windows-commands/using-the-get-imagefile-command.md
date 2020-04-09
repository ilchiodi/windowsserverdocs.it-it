---
title: Get-ImageFile
description: Windows Commands argomento per Get-ImageFile, che consente di recuperare informazioni sulle immagini contenute in un file di immagine Windows (wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef1cf2b9eec6739690d286c32d26dd84b07e348c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830994"
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
|/ImageFile:\<percorso del file WIM >|Specifica il percorso completo e il nome file del file con estensione wim.|
|[/ Dettagliate]|Restituisce tutti i metadati delle immagini da ogni immagine. Se si omette questa opzione, il comportamento predefinito è per restituire solo il nome dell'immagine, descrizione e nome file.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare le informazioni su un'immagine, digitare:
```
WDSUTIL /Get-ImageFile /ImageFile:C:\temp\install.wim
```
Per visualizzare informazioni dettagliate, digitare:
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:\\Server\Share\My Folder \install.wim /Detailed
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)