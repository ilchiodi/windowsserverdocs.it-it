---
title: New-CaptureImage
description: Argomento di riferimento per New-CaptureImage, che consente di creare una nuova immagine di acquisizione da un'immagine di avvio esistente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32c895792701630d6cfa849a298dc7a55f18a5a6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719711"
---
# <a name="new-captureimage"></a>New-CaptureImage

Crea una nuova immagine di acquisizione da un'immagine di avvio esistente. Le immagini di acquisizione sono immagini di avvio che avviano l'utilità di acquisizione di servizi di distribuzione Windows anziché avviare il programma di installazione. Quando si avvia un computer di riferimento (preparato con Sysprep) in un'immagine di acquisizione, una procedura guidata crea un'immagine di installazione del computer di riferimento e la Salva come file di immagine Windows (wim). È anche possibile aggiungere l'immagine a un supporto, ad esempio un CD, un DVD o un'unità USB, e quindi avviare un computer da tale supporto. Dopo aver creato l'immagine di installazione, è possibile aggiungere l'immagine al server per la distribuzione dell'avvio PXE. Per ulteriori informazioni, vedere Creazione di immagini[https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)().

## <a name="syntax"></a>Sintassi

```
WDSUTIL [Options] /New-CaptureImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
        /FilePath:<File path and name>
        [/Name:<Name>]
        [/Description:<Description>]
        [/Overwrite:{Yes | No | Append}]
        [/UnattendFilePath:<File path>]
```

### <a name="parameters"></a>Parametri

|        Parametro         |                                                                                                                                                                                                                         Descrizione                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<nome server>] |                                                                                                                                       Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.                                                                                                                                        |
|   /Image:\<nome immagine>   |                                                                                                                                                                                                         Specifica il nome dell'immagine di avvio di origine.                                                                                                                                                                                                         |
|   /Architettura: {x86    |                                                                                                                                                                                                                             ia64                                                                                                                                                                                                                             |
| [/Filename: \<nomefile>] |                                                                                                                                                                            Se l'immagine non può essere identificata in modo univoco dal nome, è necessario utilizzare questa opzione per specificare il nome del file.                                                                                                                                                                            |
|    /DestinationImage     | Specifica le impostazioni per l'immagine di destinazione. Specificare le impostazioni utilizzando le opzioni seguenti:</br>-/FilePath: \<percorso file e nome> imposta il percorso completo del file per la nuova immagine di acquisizione.</br>-[/Name: \<Name>]-imposta il nome visualizzato dell'immagine. Se non viene specificato alcun nome visualizzato, verrà utilizzato il nome visualizzato dell'immagine di origine.</br>-[/Description: \<Description>]-imposta la descrizione dell'immagine.</br>-[/Overwrite: {Yes |

## <a name="examples"></a>Esempi

Per creare un'immagine di acquisizione e denominarla WinPECapture. wim, digitare:
```
WDSUTIL /New-CaptureImage /Image:WinPE boot image /Architecture:x86 /DestinationImage /FilePath:C:\Temp\WinPECapture.wim
```
Per creare un'immagine di acquisizione e applicare le impostazioni specificate, digitare:
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:WinPE boot image /Architecture:x64 /Filename:boot.wim 
/DestinationImage /FilePath:\\Server\Share\WinPECapture.wim /Name:New WinPE image /Description:WinPE image with capture utility /Overwrite:No /UnattendFilePath:\\Server\Share\WDSCapture.inf
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)