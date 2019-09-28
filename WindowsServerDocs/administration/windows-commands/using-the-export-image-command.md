---
title: Utilizzando il comando Esporta immagine
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9b8b467-0f2d-4754-8998-55503a262778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 838d84cb5f604eea710c364ed0d4a14efdc16fec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363400"
---
# <a name="using-the-export-image-command"></a>Utilizzando il comando Esporta immagine

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
supporto: <Image name>|Specifica il nome dell'immagine da esportare.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
mediatype:{Boot &#124; Install}|Specifica il tipo di immagine da esportare.|
|\mediaGroup:<Image group name>]|Specifica il gruppo di immagini contenente l'immagine da esportare. Se viene specificato alcun nome di gruppo di immagini e il gruppo solo un'immagine esistente sul server, tale gruppo di immagini da utilizzare per impostazione predefinita. Se è presente più di un gruppo di immagini nel server, è necessario specificare il gruppo di immagini.|
|/ Architettura: {x86 &#124; ia64 &#124; x64}|Specifica l'architettura dell'immagine da esportare. Poiché è possibile che lo stesso nome di immagine per immagini di avvio diverse architetture, specificando il valore di architettura si garantisce che verrà restituito l'immagine corretta.|
|[/Filename:<Filename>]|Se l'immagine non può essere identificata in modo univoco in base al nome, è necessario specificare il nome del file.|
|/DestinationImage|Specifica le impostazioni per l'immagine di destinazione. È possibile specificare queste impostazioni utilizzando le opzioni seguenti:<br /><br />-/FilePath: <File path and name>-specifica il percorso completo del file per la nuova immagine.<br />-[/Name:<Name>]-imposta il nome visualizzato dell'immagine. Se viene specificato alcun nome, verrà utilizzato il nome visualizzato dell'immagine di origine.<br />-[/Description: <Description>]-Imposta la descrizione dell'immagine.|
|[/Overwrite: {Sì &#124; No &#124; aggiungere}]|Determina se il file specificato nell'opzione **/DestinationImage** verrà sovrascritto se un file esistente con lo stesso nome esiste già in/FilePath.<br /><br />-   **Sì** determina la sovrascrittura del file esistente.<br />-   **No** (opzione predefinita) causa un errore se esiste già un file con lo stesso nome.<br />-   **Append** fa in modo che l'immagine generata venga accodata come nuova immagine all'interno del file con estensione wim esistente.|
## <a name="BKMK_examples"></a>Esempi
Per esportare un'immagine di avvio, digitare uno dei seguenti:
```
wdsutil /Export-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86 /DestinationImage /Filepath:"C:\temp\boot.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filename:boot.wim 
/DestinationImage /Filepath:"\\Server\Share\ExportImage.wim" /Name:"Exported WinPE image" /Description:"WinPE Image from WDS server" /Overwrite:Yes
```
Per esportare un'immagine di installazione, digitare uno dei seguenti:
```
wdsutil /Export-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Filepath:"C:\Temp\Install.wim"
wdsutil /verbose /Progress /Export-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Filepath:\\server\share\export.wim /Name:"Exported Windows image" /Description:"Windows Vista image from WDS server" /Overwrite:append
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando Aggiungi immagine](using-the-add-image-command.md)
[utilizzando il comando Copia immagine](using-the-copy-image-command.md)
[utilizzando il comando get-immagine](using-the-get-image-command.md)
[utilizzando il comando remove-immagine](using-the-remove-image-command.md)
[utilizzando l'immagine di sostituzione comando](using-the-replace-image-command.md)
[sottocomando: set-immagine](subcommand-set-image.md)
