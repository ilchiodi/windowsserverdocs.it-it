---
title: Utilizzando il comando get-immagine
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ecaa999-72ad-4191-adb5-a418de42a001
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b78f4ed9352c21bf6de19136a625a4f4fe7ac5f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877442"
---
# <a name="using-the-get-image-command"></a>Utilizzando il comando get-immagine

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informazioni su un'immagine.
## <a name="syntax"></a>Sintassi
per immagini d'avvio:
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:Boot /Architecture:{x86 | ia64 | x64} [/Filename:<File name>]
```
per immagini di installazione:
```
wdsutil [Options] /Get-Imagmedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
media:<Image name>|Specifica il nome dell'immagine.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
mediatype:{Boot &#124; Install}|Specifica il tipo di immagine.|
|/ Architettura: {x86 & #124; ia64 & #124; x64}|Specifica l'architettura dell'immagine. Poiché è possibile avere lo stesso nome di immagine per immagini di avvio diverse architetture, specificando il valore di architettura garantisce che venga restituito l'immagine corretta.|
|[/Filename:<File name>]|Se l'immagine non può essere identificata dal nome, è necessario utilizzare questa opzione per specificare il nome del file.|
|\mediaGroup:<Image group name>]|Specifica il gruppo di immagini che contiene l'immagine. Se non viene specificato alcun gruppo di immagini e gruppo solo un'immagine esistente sul server, verrà utilizzato tale gruppo. Se è presente più di un gruppo di immagini nel server, è necessario utilizzare questo parametro per specificare il gruppo di immagini.|
## <a name="BKMK_examples"></a>Esempi
Per recuperare informazioni su un'immagine di avvio, digitare uno dei seguenti:
```
wdsutil /Get-Imagmedia:"WinPE boot imagemediatype:Boot /Architecture:x86
wdsutil /verbose /Get-Imagmedia:"WinPE boot image" /Server:MyWDSServemediatype:Boot /Architecture:x86 /Filename:boot.wim
```
Per recuperare informazioni su un'immagine di installazione, digitare uno dei seguenti:
```
wdsutil /Get-Imagmedia:"Windows Vista with Officemediatype:Install
wdsutil /verbose /Get-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando add-immagine](using-the-add-image-command.md)
[utilizzando il comando Copia immagine](using-the-copy-image-command.md)
[tramite il Comando Export-Image](using-the-export-image-command.md)
[utilizzando il comando remove-immagine](using-the-remove-image-command.md)
[usando l'immagine di sostituzione comando](using-the-replace-image-command.md) 
 [Sottocomando: set-immagine](subcommand-set-image.md)
