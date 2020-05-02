---
title: Sostituisci immagine
description: Argomento di riferimento per Replace-Image, che sostituisce un'immagine esistente con una nuova versione dell'immagine.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68ded3df-e309-420f-9f5d-caeb609385a5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9dad35e54f064e02b863059ae6da9378403ee4f9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720324"
---
# <a name="using-the-replace-image-command"></a>Utilizzando l'immagine di sostituzione comando

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sostituisce un'immagine esistente con una nuova versione dell'immagine.
## <a name="syntax"></a>Sintassi
per le immagini di avvio:
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/Name:<Image name>]
         [/Description:<Image description>]
```
per le immagini di installazione:
```
wdsutil [Options] /replace-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /replacementImage
       mediaFile:<wim file path>
         [/SourceImage:<Source image name>]
         [/Name:<Image name>]
         [/Description:<Image description>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
media<Image name>|Specifica il nome dell'immagine da sostituire.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
MediaType: {boot &#124; install}|Specifica il tipo di immagine da sostituire.|
|/ Architettura: {x86 & #124; ia64 & #124; x64}|Specifica l'architettura dell'immagine da sostituire. Poiché è possibile avere lo stesso nome di immagine per immagini di avvio diverse in architetture diverse, specificando l'architettura si garantisce che venga sostituita l'immagine corretta.|
|[/Filename:<File name>]|Se l'immagine non può essere identificata in modo univoco in base al nome, è necessario utilizzare questa opzione per specificare il nome del file.|
|/replacementImage|Specifica le impostazioni per l'immagine di sostituzione. È possibile impostare queste impostazioni usando le opzioni seguenti:<p>-mediaFile: <file path> -specifica il nome e il percorso (percorso completo) del nuovo file con estensione wim.<br />-[/SourceImage: <image name>]-specifica l'immagine da utilizzare se il file con estensione wim contiene più immagini. Questa opzione si applica solo alle immagini di installazione.<br />-[/Name:<Image name>] imposta il nome visualizzato dell'immagine.<br />-[/Description:<Image description>]-imposta la descrizione dell'immagine.|
## <a name="examples"></a>Esempi
Per sostituire un'immagine di avvio, digitare uno dei seguenti:
```
wdsutil /replace-Imagmedia:WinPE Boot Imagemediatype:Boot /Architecture:x86 /replacementImagmediaFile:C:\MyFolder\Boot.wim
wdsutil /verbose /Progress /replace-Imagmedia:WinPE Boot Image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/replacementImagmediaFile:\\MyServer\Share\Boot.wim /Name:My WinPE Image /Description:WinPE Image with drivers
```
Per sostituire un'immagine di installazione, digitare uno dei seguenti:
```
wdsutil /replace-Imagmedia:Windows Vista Homemediatype:Install /replacementImagmediaFile:C:\MyFolder\Install.wim
wdsutil /verbose /Progress /replace-Imagmedia:Windows Vista Pro /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:Install.wim /replacementImagmediaFile:\\MyServer\Share \Install.wim /SourceImage:Windows Vista Ultimate /Name:Windows Vista Desktop /Description:Windows Vista Ultimate with standard business applications.
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando usando il comando[Add-Image](using-the-add-image-command.md)
usando
[il comando copy-image](using-the-copy-image-command.md)
[usando il comando Export-Image](using-the-export-image-command.md)usando il comando[Get-Image](using-the-get-image-command.md)

[usando il comando Replace-Image](using-the-replace-image-command.md)[sottocomando: Set-Image](subcommand-set-image.md)
