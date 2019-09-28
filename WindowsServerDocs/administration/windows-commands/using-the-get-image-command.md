---
title: Utilizzando il comando get-immagine
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 73b25fd70c1512f99b5097b2317c3f0403f3526c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363111"
---
# <a name="using-the-get-image-command"></a>Utilizzando il comando get-immagine

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
supporto: <Image name>|Specifica il nome dell'immagine.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
mediatype:{Boot &#124; Install}|Specifica il tipo di immagine.|
|/ Architettura: {x86 &#124; ia64 &#124; x64}|Specifica l'architettura dell'immagine. Poiché è possibile avere lo stesso nome di immagine per le immagini di avvio in diverse architetture, specificando il valore dell'architettura si garantisce che venga restituita l'immagine corretta.|
|[/Filename:<File name>]|Se l'immagine non può essere identificata in modo univoco in base al nome, è necessario utilizzare questa opzione per specificare il nome del file.|
|\mediaGroup:<Image group name>]|Specifica il gruppo di immagini che contiene l'immagine. Se non viene specificato alcun gruppo di immagini e nel server è presente un solo gruppo di immagini, verrà utilizzato tale gruppo. Se nel server è presente più di un gruppo di immagini, è necessario usare questo parametro per specificare il gruppo di immagini.|
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
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
 usando il comando[add-image](using-the-add-image-command.md)
 usando il comando[Copy-Image](using-the-copy-image-command.md)
 usando il comando[Export-Image](using-the-export-image-command.md)
[usando il comando Remove-Image](using-the-remove-image-command.md)
[usando il comando Comando Replace-Image](using-the-replace-image-command.md)1[sottocomando: Set-Image](subcommand-set-image.md)
