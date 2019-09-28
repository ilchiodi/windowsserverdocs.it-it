---
title: Utilizzando il comando Aggiungi immagine
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5b6f4da-90ba-4b0e-9423-66c8ef5172e2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0d671dd482710c486a6936cdbe3b1cc6b331866
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363744"
---
# <a name="using-the-add-image-command"></a>Utilizzando il comando Aggiungi immagine

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

aggiunge immagini a un server di servizi di distribuzione Windows. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_examples).
## <a name="syntax"></a>Sintassi
per le immagini di avvio, usare la sintassi seguente:
```
wdsutil /add-ImagmediaFile:<wim file path> [/Server:<Server name>mediatype:Boot [/Skipverify] [/Name:<Image name>] [/Description:<Image description>] 
[/Filename:<New wim file name>]
```
per le immagini di installazione, usare la sintassi seguente:
```
wdsutil /add-ImagmediaFile:<wim file path>
     [/Server:<Server name>]
   mediatype:Install
     [/Skipverify]
    mediaGroup:<Image group name>]
     [/SingleImage:<Single image name>]
         [/Name:<Name>]
         [/Description:<Description>]
     [/Filename:<File name>]
     [/UnattendFile:<Unattend file path>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
mediaFile: < percorso del file con estensione wim >|Specifica il nome di file e percorso completo del file di immagine Windows (wim) che contiene le immagini da aggiungere.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se non si specifica alcun nome server, verrà utilizzato il server locale.|
mediatype:{Boot&#124;Install}|Specifica il tipo di immagini da aggiungere.|
|[/Skipverify]|Specifica che verifica integrità non verrà eseguita sul file di immagine di origine prima di aggiungere l'immagine.|
|[/Name:<Name>]|Imposta il nome visualizzato dell'immagine.|
|/Description<Description>]|Imposta la descrizione dell'immagine.|
|[/Filename:<Filename>]|Specifica il nuovo nome di file per il file con estensione wim. In questo modo è possibile modificare il nome del file del file con estensione wim quando si aggiunge l'immagine. Se viene specificato alcun nome di file, verrà utilizzato il nome del file di immagine di origine. In tutti i casi, i servizi di distribuzione Windows verifica per determinare se il nome del file è univoco nell'archivio di immagini di avvio del computer di destinazione.|
|\mediaGroup:<Image group name>]|Specifica il nome del gruppo di immagini in cui le immagini devono essere aggiunti. Se è presente più di un gruppo di immagini nel server, è necessario specificare il gruppo di immagini. Se non si specifica alcun gruppo di immagini e un gruppo di immagini non esiste già, verrà creato un nuovo gruppo di immagini. In caso contrario, verrà utilizzato il gruppo di immagini esistente.|
|[/SingleImage: <Single image name>] [/Name: <Name>] /Description<Description>]|Copia l'immagine singola specificata da un file con estensione wim e imposta il nome dell'immagine e descrizione.|
|[/UnattendFile:<Unattend file path>]|Specifica il percorso completo al file di installazione automatica da associare le immagini che vengono aggiunte. Se **/SingleImage** non è specificato, lo stesso file di installazione automatica verrà associato a tutte le immagini nel file con estensione wim.|
## <a name="BKMK_examples"></a>Esempi
Per aggiungere un'immagine di avvio, digitare:
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Boot.wimmediatype:Boot
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share\Boot.wim /Server:MyWDSServemediatype:Boot /Name:"My WinPE Image" 
/Description:"WinPE Image containing the WDS Client" /Filename:WDSBoot.wim
```
Per aggiungere un'immagine di installazione, digitare uno dei seguenti:
```
wdsutil /add-ImagmediaFile:"C:\MyFolder\Install.wimmediatype:Install
wdsutil /verbose /Progress /add-ImagmediaFile:\\MyServer\Share \Install.wim /Server:MyWDSServemediatype:InstalmediaGroup:ImageGroup1 
/SingleImage:"Windows Pro" /Name:"My WDS Image"
/Description:"Windows Pro image with Microsoft Office" /Filename:"Win Pro.wim" /UnattendFile:"\\server\share\unattend.xml"
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando Copia immagine](using-the-copy-image-command.md)
[utilizzando il comando Export-Image](using-the-export-image-command.md)
[utilizzando il comando get-immagine](using-the-get-image-command.md)
[utilizzando il comando remove-immagine](using-the-remove-image-command.md)
[utilizzando l'immagine di sostituzione comando](using-the-replace-image-command.md)
[sottocomando: set-immagine](subcommand-set-image.md)
