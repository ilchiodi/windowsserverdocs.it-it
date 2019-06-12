---
title: Utilizzando il comando nuovo CaptureImage
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00f15ae7ee1a7ab1ac1f71599d2cae9bb51d921e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440410"
---
# <a name="using-the-new-captureimage-command"></a>Utilizzando il comando nuovo CaptureImage



Crea una nuova immagine di acquisizione da un'immagine di avvio esistente. Le immagini di acquisizione sono utilità invece di avviare il programma di installazione di acquisizione immagini d'avvio che avvia i servizi di distribuzione Windows. Quando si avvia un computer di riferimento (che è stato preparato con Sysprep) in un'immagine di acquisizione, una procedura guidata crea un'immagine di installazione del computer di riferimento e lo salva come un file immagine Windows (wim). È anche possibile aggiungere l'immagine di supporti (ad esempio un'unità CD, DVD o USB) e quindi avviare un computer da tale supporto. Dopo aver creato l'immagine di installazione, è possibile aggiungere l'immagine al server per la distribuzione dell'avvio PXE. Per altre informazioni, vedere Creazione di immagini ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

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

## <a name="parameters"></a>Parametri

|        Parametro         |                                                                                                                                                                                                                         Descrizione                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/ Server:\<nome Server >] |                                                                                                                                       Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.                                                                                                                                        |
|   / Image:\<nome immagine >   |                                                                                                                                                                                                         Specifica il nome dell'immagine di avvio di origine.                                                                                                                                                                                                         |
|   / Architettura: {x86    |                                                                                                                                                                                                                             ia64                                                                                                                                                                                                                             |
| [/ Nome file: \<Nome file >] |                                                                                                                                                                            Se l'immagine non può essere identificata in modo univoco dal nome, è necessario utilizzare questa opzione per specificare il nome del file.                                                                                                                                                                            |
|    /DestinationImage     | Specifica le impostazioni per l'immagine di destinazione. Specificare le impostazioni utilizzando le opzioni seguenti:</br>-   /FilePath: \<Percorso e nome file > imposta il percorso completo del file per la nuova immagine di acquisizione.</br>-[/Name: \<Nome >]-imposta il nome visualizzato dell'immagine. Se non viene specificato alcun nome visualizzato, verrà utilizzato il nome visualizzato dell'immagine di origine.</br>-[/ Descrizione: \<Descrizione >]-imposta la descrizione dell'immagine.</br>-[/Overwrite: {Sì |

## <a name="BKMK_examples"></a>Esempi

Per creare un'immagine di acquisizione e denominarlo WinPECapture.wim, digitare:
```
WDSUTIL /New-CaptureImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPECapture.wim"
```
Per creare un'immagine di acquisizione e applicare le impostazioni specificate, digitare:
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim 
/DestinationImage /FilePath:"\\Server\Share\WinPECapture.wim" /Name:"New WinPE image" /Description:"WinPE image with capture utility" /Overwrite:No /UnattendFilePath:"\\Server\Share\WDSCapture.inf"
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)