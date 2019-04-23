---
title: Utilizzando il comando get-ImageFile
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6bbe5ece95d1f9821a27b96e56bc34576a0f5f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827622"
---
# <a name="using-the-get-imagefile-command"></a>Utilizzando il comando get-ImageFile



Recupera le informazioni sulle immagini contenuta in un file immagine Windows (wim).

## <a name="syntax"></a>Sintassi

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/ ImageFile:\<percorso del file WIM >|Specifica il nome di file e percorso completo del file con estensione wim.|
|[/ Dettagliate]|Restituisce tutti i metadati delle immagini da ogni immagine. Se si omette questa opzione, il comportamento predefinito è per restituire solo il nome dell'immagine, descrizione e nome file.|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare informazioni su un'immagine, digitare:
```
WDSUTIL /Get-ImageFile /ImageFile:"C:\temp\install.wim"
```
Per visualizzare informazioni dettagliate, digitare:
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:"\\Server\Share\My Folder \install.wim" /Detailed
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)