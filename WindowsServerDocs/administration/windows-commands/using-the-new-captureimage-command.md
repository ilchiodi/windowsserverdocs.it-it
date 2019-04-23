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
ms.openlocfilehash: 7d9f402acb9904624bdb4193a4306d57b104eda8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888612"
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

|Parametro|Descrizione|
|---------|-----------|
|[/ Server:\<nome Server >]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/ Image:\<nome immagine >|Specifica il nome dell'immagine di avvio di origine.|
|/ Architettura: {x86 | ia64 | x64}|Specifica l'architettura dell'immagine da usare. Poiché si può avere lo stesso nome di immagine per immagini di avvio diverse architetture, specificando in questo modo l'immagine corretta verrà utilizzato.|
|[/ Nome file: \<Nome file >]|Se l'immagine non può essere identificata in modo univoco dal nome, è necessario utilizzare questa opzione per specificare il nome del file.|
|/DestinationImage|Specifica le impostazioni per l'immagine di destinazione. Specificare le impostazioni utilizzando le opzioni seguenti:</br>-   /FilePath: \<Percorso e nome file > imposta il percorso completo del file per la nuova immagine di acquisizione.</br>-[/Name: \<Nome >]-imposta il nome visualizzato dell'immagine. Se non viene specificato alcun nome visualizzato, verrà utilizzato il nome visualizzato dell'immagine di origine.</br>-[/ Descrizione: \<Descrizione >]-imposta la descrizione dell'immagine.</br>-[/Overwrite: {Sì | No | Aggiungere}] - determina se il file specificato nel **/DestinationImage** devono essere sovrascritti se in /FilePath esiste già un altro file con lo stesso nome. **Sì** sovrascrive il file esistente. **Non** (impostazione predefinita) causa un errore si verifica se un altro file con lo stesso nome esiste già. **Aggiungere** Collega l'immagine generata come una nuova immagine all'interno del file con estensione wim esistente.</br>-   [/UnattendFilePath: \<Percorso del file >]-imposta il percorso completo e nome del file di acquisizione immagine automatica.|

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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)