---
title: New-DiscoverImage
description: Argomento di riferimento per New-DiscoverImage, che consente di creare una nuova immagine di individuazione da un'immagine di avvio esistente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ede9fbbb-0bba-4309-8c21-3cc13e1dc3cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45202f2bde3ca045a27e604743c25659df2436f9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725792"
---
# <a name="new-discoverimage"></a>New-DiscoverImage

Crea una nuova immagine di individuazione da un'immagine di avvio esistente. Individuare le immagini sono immagini di avvio che impongono il programma Setup.exe di avvio in modalità servizi di distribuzione Windows e quindi individuare un server di servizi di distribuzione Windows. Queste immagini vengono generalmente utilizzate per distribuire le immagini ai computer che non sono in grado di consentire l'avvio di PXE. Per ulteriori informazioni, vedere Creazione di immagini[https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)().

## <a name="syntax"></a>Sintassi

```
WDSUTIL [Options] /New-DiscoverImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
         /FilePath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
         [/WDSServer:<Server name>]
         [/Overwrite:{Yes | No | Append}]
```

### <a name="parameters"></a>Parametri

|        Parametro         |                                                                                                                                                                                                                                                                                                                                                                                                                       Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                       |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<nome server>] |                                                                                                                                                                                                                                                                                                                                     Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.                                                                                                                                                                                                                                                                                                                                     |
|   /Image:\<nome immagine>   |                                                                                                                                                                                                                                                                                                                                                                                                      Specifica il nome dell'immagine di avvio di origine.                                                                                                                                                                                                                                                                                                                                                                                                       |
|    /Architettura: {x86    |                                                                                                                                                                                                                                                                                                                                                                                                                          ia64                                                                                                                                                                                                                                                                                                                                                                                                                           |
| [/Filename:\<nome file>] |                                                                                                                                                                                                                                                                                                                                                                         Se l'immagine non può essere identificata in modo univoco dal nome, è necessario utilizzare questa opzione per specificare il nome del file.                                                                                                                                                                                                                                                                                                                                                                          |
|    /DestinationImage     | Specifica le impostazioni per l'immagine di destinazione. È possibile specificare le impostazioni utilizzando le opzioni seguenti:</br>-/FilePath: < percorso e nome file > - Imposta percorso completo del file per la nuova immagine.</br>-[/Name:\<Name>]-imposta il nome visualizzato dell'immagine. Se non viene specificato alcun nome visualizzato, verrà utilizzato il nome visualizzato dell'immagine di origine.</br>-[/Description: \<Description>]-imposta la descrizione dell'immagine.</br>-[/WDSServer: \<nome server>]-specifica il nome del server che tutti i client che eseguono l'avvio dall'immagine specificata devono contattare per scaricare l'immagine di installazione. Per impostazione predefinita, tutti i client che eseguono l'avvio di questa immagine individuerà un server di servizi di distribuzione Windows valido. Questa opzione Ignora la funzionalità di individuazione e forza il client avviato contattare il server specificato.</br>-[/Overwrite: {Yes |

## <a name="examples"></a>Esempi

Per creare un'immagine di individuazione all'immagine di avvio esterno e denominarlo WinPEDiscover.wim, digitare:
```
WDSUTIL /New-DiscoverImage /Image:WinPE boot image /Architecture:x86 /DestinationImage /FilePath:C:\Temp\WinPEDiscover.wim
```
Per creare un'immagine di individuazione all'immagine di avvio esterno e denominarlo WinPEDiscover.wim con le impostazioni specificate, digitare:
```
WDSUTIL /Verbose /Progress /New-DiscoverImage /Server:MyWDSServer
/Image:WinPE boot image /Architecture:x64 /Filename:boot.wim /DestinationImage /FilePath:\\Server\Share\WinPEDiscover.wim 
/Name:New WinPE image /Description:WinPE image for WDS Client discovery /Overwrite:No
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)