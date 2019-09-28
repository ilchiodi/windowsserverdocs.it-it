---
title: Utilizzando il comando get-ImageFile
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c8136585e04caca02ab16c7b4ca11a825cf400d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392141"
---
# <a name="using-the-get-imagefile-command"></a>Utilizzando il comando get-ImageFile



Recupera le informazioni sulle immagini contenute in un file di immagine Windows (wim).

## <a name="syntax"></a>Sintassi

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/ImageFile: percorso file \<WIM >|Specifica il percorso completo e il nome file del file con estensione wim.|
|[/ Dettagliate]|Restituisce tutti i metadati delle immagini da ogni immagine. Se si omette questa opzione, il comportamento predefinito è per restituire solo il nome dell'immagine, descrizione e nome file.|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare le informazioni su un'immagine, digitare:
```
WDSUTIL /Get-ImageFile /ImageFile:"C:\temp\install.wim"
```
Per visualizzare informazioni dettagliate, digitare:
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:"\\Server\Share\My Folder \install.wim" /Detailed
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)