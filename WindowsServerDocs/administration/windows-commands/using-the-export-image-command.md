---
title: Export-Image
description: Argomento di riferimento per Export-Image, che esporta un'immagine esistente dall'archivio immagini a un altro file di immagine Windows (wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 477859687732fa2cccdc782edb81fdd348f313c5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720906"
---
# <a name="export-image"></a>Export-Image

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esporta un'immagine esistente dall'archivio di immagini in un altro file di immagine Windows (wim).

## <a name="syntax"></a>Sintassi
per le immagini di avvio:
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No}]
```
per le immagini di installazione:
```
wdsutil [Options] /Export-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:InstallmediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Filepath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
     [/Overwrite:{Yes | No | append}]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
media<Image name>|Specifica il nome dell'immagine da esportare.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
MediaType: {boot &#124; install}|Specifica il tipo di immagine da esportare.|
|\mediaGroup:<Image group name>]|Specifica il gruppo di immagini contenente l'immagine da esportare. Se viene specificato alcun nome di gruppo di immagini e il gruppo solo un'immagine esistente sul server, tale gruppo di immagini da utilizzare per impostazione predefinita. Se è presente più di un gruppo di immagini nel server, è necessario specificare il gruppo di immagini.|
|/ Architettura: {x86 & #124; ia64 & #124; x64}|Specifica l'architettura dell'immagine da esportare. Poiché è possibile che lo stesso nome di immagine per immagini di avvio diverse architetture, specificando il valore di architettura si garantisce che verrà restituito l'immagine corretta.|
|[/Filename:<Filename>]|Se l'immagine non può essere identificata in modo univoco in base al nome, è necessario specificare il nome del file.|
|/DestinationImage|Specifica le impostazioni per l'immagine di destinazione. È possibile specificare queste impostazioni utilizzando le opzioni seguenti:<p>-/FilePath:<File path and name> -specifica il percorso completo del file per la nuova immagine.<br />-[/Name:<Name>]-imposta il nome visualizzato dell'immagine. Se viene specificato alcun nome, verrà utilizzato il nome visualizzato dell'immagine di origine.<br />-[/Description: <Description>]-Imposta la descrizione dell'immagine.|
|[/Overwrite: {Yes &#124; no &#124; Append}]|Determina se il file specificato nell'opzione **/DestinationImage** verrà sovrascritto se un file esistente con lo stesso nome esiste già in/FilePath.<p>-   **Sì** determina la sovrascrittura del file esistente.<br />-   **No** (opzione predefinita) causa un errore se esiste già un file con lo stesso nome.<br />-   **Append** fa in modo che l'immagine generata venga accodata come nuova immagine all'interno del file con estensione wim esistente.|
## <a name="examples"></a>Esempi
Per esportare un'immagine di avvio, digitare uno dei seguenti:
```
wdsutil /Export-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86 /DestinationImage /Filepath:C:\temp\boot.wim
wdsutil /verbose /Progress /Export-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/DestinationImage /Filepath:\\Server\Share\ExportImage.wim /Name:Exported WinPE image /Description:WinPE Image from WDS server /Overwrite:Yes
```
Per esportare un'immagine di installazione, digitare uno dei seguenti:
```
wdsutil /Export-Imagmedia:Windows Vista with Officemediatype:Install /DestinationImage /Filepath:C:\Temp\Install.wim
wdsutil /verbose /Progress /Export-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:Exported Windows image /Description:Windows Vista image from WDS server /Overwrite:append
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando usando il comando[Add-Image](using-the-add-image-command.md)
usando
[il comando copy-image](using-the-copy-image-command.md)usando il comando[Get-](using-the-get-image-command.md)
Image usando il
[comando Remove-Image](using-the-remove-image-command.md)
[usando il comando Replace-Image](using-the-replace-image-command.md)[sottocomando: Set-Image](subcommand-set-image.md)
