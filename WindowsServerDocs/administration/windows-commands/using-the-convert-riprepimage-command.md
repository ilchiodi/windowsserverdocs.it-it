---
title: Uso del comando Convert-RiprepImage
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0b5e75a4148359db5088e7ff60d29b8d157309d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363654"
---
# <a name="using-the-convert-riprepimage-command"></a>Uso del comando Convert-RiprepImage



Converte un'immagine di preparazione dell'installazione remota (RIPrep) esistente nel formato di immagine Windows (wim).

## <a name="syntax"></a>Sintassi

```
WDSUTIL [Options] /Convert-RIPrepImage /FilePath:<File path and name>
     /DestinationImage
         /FilePath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
         [/InPlace]
         [/Overwrite:{Yes | No | Append}]
```

## <a name="parameters"></a>Parametri

|            Parametro            |                                                                                                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                                                                                                                |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /FilePath: nome e percorso \<File > |                                                                                                                                                                                                       Specifica il nome di file e percorso completo del file. sif che corrisponde all'immagine RIPrep. Questo file viene in genere chiamato sif e viene trovato nella sottocartella \Templates della cartella che contiene l'immagine RIPrep.                                                                                                                                                                                                       |
|        /DestinationImage        | Specifica le impostazioni per l'immagine di destinazione, utilizzando le opzioni seguenti.</br>-/FilePath: \<File percorso e nome >-Imposta il percorso completo del file per il nuovo file. Esempio: **C:\Temp\convert.wim**</br>-[/Name: \<Name >]-imposta il nome visualizzato dell'immagine. Se non viene specificato alcun nome visualizzato, verrà utilizzato il nome visualizzato dell'immagine di origine.</br>-[/Description: \<Description >]-imposta la descrizione dell'immagine.</br>-[/InPlace] - Specifica che la conversione deve essere eseguito su immagine RIPrep originale e non su una copia dell'immagine originale, ovvero il comportamento predefinito.</br>-[/Overwrite: {Yes |

## <a name="BKMK_examples"></a>Esempi

Per convertire l'immagine specificata sif RIPREP.wim, digitare:
```
WDSUTIL /Convert-RiPrepImage /FilePath:"R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif" /DestinationImage
/FilePath:"C:\Temp\RIPREP.wim"
```
Per convertire l'immagine specificata sif RIPREP.wim con il nome specificato e la descrizione e sovrascriverlo con il nuovo file se esiste già un file, digitare:
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:"\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif"
/DestinationImage /FilePath:"\\Server\Share\RIPREP.wim"
/Name:"WindowsXP image" /Description:"Converted RIPREP image of WindowsXP"
/Overwrite:Append
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)