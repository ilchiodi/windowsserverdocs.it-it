---
title: Get-Image
description: Argomento di riferimento per Get-Image, che recupera le informazioni relative a un'immagine.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 04cc7b8d90415e32be4103ef6c7f7b709c3550c3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719916"
---
# <a name="get-image"></a>Get-Image

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera le informazioni relative a un'immagine.

## <a name="syntax"></a>Sintassi
per le immagini di avvio:
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
per le immagini di installazione:
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
media<Image name>|Specifica il nome dell'immagine.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
MediaType: {boot &#124; install}|Specifica il tipo di immagine.|
|/ Architettura: {x86 & #124; ia64 & #124; x64}|Specifica l'architettura dell'immagine. Poiché è possibile avere lo stesso nome di immagine per le immagini di avvio in diverse architetture, specificando il valore dell'architettura si garantisce che venga restituita l'immagine corretta.|
|[/Filename:<File name>]|Se l'immagine non può essere identificata in modo univoco in base al nome, è necessario utilizzare questa opzione per specificare il nome del file.|
|\mediaGroup:<Image group name>]|Specifica il gruppo di immagini che contiene l'immagine. Se non viene specificato alcun gruppo di immagini e nel server è presente un solo gruppo di immagini, verrà utilizzato tale gruppo. Se nel server è presente più di un gruppo di immagini, è necessario usare questo parametro per specificare il gruppo di immagini.|
## <a name="examples"></a>Esempi
Per recuperare informazioni su un'immagine di avvio, digitare uno dei seguenti:
```
wdsutil /Get-Imagmedia:WinPE boot imagemediatype:Boot /Architecture:x86
wdsutil /verbose /Get-Imagmedia:WinPE boot image /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
```
Per recuperare informazioni su un'immagine di installazione, digitare uno dei seguenti:
```
wdsutil /Get-Imagmedia:Windows Vista with Officemediatype:Install
wdsutil /verbose /Get-Imagmedia:Windows Vista with Office /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando usando il comando[Add-Image](using-the-add-image-command.md)
usando
[il comando copy-](using-the-copy-image-command.md)Image[usando il comando Export-Image](using-the-export-image-command.md)
usando il[comando](using-the-remove-image-command.md)
Remove-Image[usando il comando](using-the-replace-image-command.md)
Replace-Image[sottocomando: Set-Image](subcommand-set-image.md)
