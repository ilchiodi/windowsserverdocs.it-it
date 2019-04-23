---
title: Utilizzando il comando Copia immagine
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bea41cf4-36e6-4181-afa5-00170ebd4fdc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 52c9c0bb45e60e76077bf90534e93f2c6fc1df18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884042"
---
# <a name="using-the-copy-image-command"></a>Utilizzando il comando Copia immagine

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia le immagini appartenenti allo stesso gruppo di immagini. Per copiare immagini tra gruppi di immagini, utilizzare il [utilizzando il comando Export-Image](using-the-export-image-command.md) comando e quindi la [utilizzando il comando add-Image](using-the-add-image-command.md) comando.
Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_examples).
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /copy-Imagmedia:<Image name> [/Server:<Server name>]
   mediatype:Install
    mediaGroup:<Image group name>]
     [/Filename:<File name>]
     /DestinationImage
         /Name:<Name>
         /Filename:<File name>
         [/Description:<Description>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
media:<Image name>|Specifica il nome dell'immagine da copiare.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
MediaType:Install|Specifica il tipo di immagine da copiare. Questa opzione deve essere impostata su **installare**.|
|\mediaGroup:<Image group name>]|Specifica il gruppo di immagini che contiene l'immagine da copiare. Se viene specificato alcun gruppo di immagini e un solo gruppo esiste nel server, tale gruppo di immagini da utilizzare per impostazione predefinita. Se nel server è presente più di un gruppo di immagini, è necessario specificare il gruppo desiderato.|
|[/Filename:<Filename>]|Specifica il nome del file dell'immagine da copiare. Se l'immagine di origine non può essere identificata in modo univoco dal nome, è necessario specificare il nome del file.|
|/DestinationImage|Specifica le impostazioni per l'immagine di destinazione, come descritto nella tabella seguente.<br /><br />-/Name:<Name> -Imposta il nome visualizzato dell'immagine da copiare.<br />-/Filename:<Filename> -Imposta il nome del file di immagine di destinazione che conterrà la copia dell'immagine.<br />-[/ Descrizione: <Description>]-Imposta la descrizione della copia immagine.|
## <a name="BKMK_examples"></a>Esempi
Per creare una copia dell'immagine specificata e denominarlo WindowsVista.wim, digitare:
```
wdsutil /copy-Imagmedia:"Windows Vista with Officemediatype:Install /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim"
```
Per creare una copia dell'immagine specificata, applicare le impostazioni specificate e denominarla WindowsVista.wim, tipo:
```
wdsutil /verbose /Progress /copy-Imagmedia:"Windows Vista with Office" /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/Filename:install.wim /DestinationImage /Name:"copy of Windows Vista with Office" /Filename:"WindowsVista.wim" /Description:"This is a copy of the original Windows image with Office installed"
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando add-immagine](using-the-add-image-command.md)
[utilizzando il comando Export-Image](using-the-export-image-command.md)
[utilizzando il comando get-immagine](using-the-get-image-command.md)
[utilizzando il comando remove-immagine](using-the-remove-image-command.md)
[utilizzando l'immagine di sostituzione comando](using-the-replace-image-command.md)
[sottocomando: set-immagine](subcommand-set-image.md)
