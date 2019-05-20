---
title: Il sottocomando start-MulticastTransmission
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1b2d459-1ece-49d4-997c-9d206c463b61
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7e3e59a0907caf2769d5df00aeaf00589ab450d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842682"
---
# <a name="subcommand-start-multicasttransmission"></a>Sottocomando: start-MulticastTransmission

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avvia una trasmissione multicast pianificato di un'immagine.
## <a name="syntax"></a>Sintassi
**Windows Server 2008**
```
wdsutil /start-MulticastTransmissiomedia:<Image name> [/Server:<Server namemediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
**Windows Server 2008 R2** per immagini d'avvio:
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
per immagini di installazione:
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
media:<Image name>|Specifica il nome dell'immagine.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
mediatype:{Install&#124;Boot}|Specifica il tipo di immagine. Si noti che questa opzione deve essere impostata su **installare** per Windows Server 2008.|
|/ Architettura: {x86 &#124; ia64 &#124; x64}|L'architettura dell'immagine di avvio associata per avviare la trasmissione. Poiché è possibile che lo stesso nome di immagine per immagini di avvio diverse architetture, è necessario specificare l'architettura per garantire la corretta trasmissione viene utilizzata.|
|\mediaGroup:<Image group name>]|Specifica il gruppo di immagini dell'immagine. Se viene specificato alcun nome di gruppo di immagini e il gruppo solo un'immagine esistente sul server, verrà utilizzato tale gruppo di immagini. Se è presente più di un gruppo di immagini nel server, è necessario utilizzare questa opzione per specificare il nome del gruppo di immagini.|
|[/Filename:<File name>]|Specifica il nome del file che contiene l'immagine. Se l'immagine non può essere identificata in modo univoco dal nome, è necessario utilizzare questa opzione per specificare il nome del file.|
## <a name="BKMK_examples"></a>Esempi
Per avviare una trasmissione multicast, digitare uno dei seguenti:
```
wdsutil /start-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
Per un avvio la trasmissione multicast per Windows Server 2008 R2, tipo di immagine:
```
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"X64 Boot Imagemediatype:Boot /Architecture:x64
/Filename:boot.wim\n\
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[utilizzando il comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
[utilizzando il comando /New-multicasttransmission](using-the-new-multicasttransmission-command.md)
[utilizzando il comando remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
