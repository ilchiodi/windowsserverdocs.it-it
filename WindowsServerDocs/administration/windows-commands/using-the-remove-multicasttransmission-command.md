---
title: Utilizzando il comando remove-MulticastTransmission
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 279554124b046f645b3c83e1490657aa8782104a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362823"
---
# <a name="using-the-remove-multicasttransmission-command"></a>Utilizzando il comando remove-MulticastTransmission

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disabilita la trasmissione multicast per un'immagine. A meno che non si specifichi **/Force**, i client esistenti completeranno il trasferimento di immagini, ma non sarà possibile aggiungere nuovi client.
## <a name="syntax"></a>Sintassi
**Windows Server 2008**
```
wdsutil /remove-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image Group>] [/Filename:<File name>] [/force]
```
**Windows Server 2008 R2** per le immagini d'avvio:
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
\x20    [/Server:<Server name>]
\x20  mediatype:Boot
\x20    /Architecture:{x86 | ia64 | x64}
\x20    [/Filename:<File name>]
```
per le immagini di installazione:
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
supporto: <Image name>|Specifica il nome dell'immagine.|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di server, viene utilizzato il server locale.|
mediatype:{Install&#124;Boot}|Specifica il tipo di immagine. Si noti che questa opzione deve essere impostata su **installare** per Windows Server 2008.|
|/ Architettura: {x86 &#124; ia64 &#124; x64}|Specifica l'architettura dell'immagine di avvio associata per avviare la trasmissione. Poiché è possibile che lo stesso nome di immagine per immagini di avvio diverse architetture, è necessario specificare l'architettura per garantire la corretta trasmissione viene utilizzata.|
|\mediaGroup:<Image group name>]|Specifica il gruppo di immagini che contiene l'immagine. Se viene specificato alcun nome di gruppo di immagini e il gruppo solo un'immagine esistente sul server, viene utilizzato il gruppo di immagini. Se è presente più di un gruppo di immagini nel server, è necessario utilizzare questa opzione per specificare il nome del gruppo di immagini.|
|[/Filename:<File name>]|Specifica il nome del file. Se l'immagine di origine non può essere identificata in modo univoco dal nome, è necessario utilizzare questa opzione per specificare il nome del file.|
|/Force|rimuove la trasmissione e termina tutti i client. A meno che non si specifichi un valore per l'opzione **/Force** , i client esistenti possono completare il trasferimento dell'immagine ma non è possibile aggiungere nuovi client.|
## <a name="BKMK_examples"></a>Esempi
Per arrestare uno spazio dei nomi (client corrente verrà completata la trasmissione, ma i nuovi client non sarà in grado di creare un join), tipo:
```
wdsutil /remove-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmissiomedia:"x64 Boot Image"
/Imagetype:Boot /Architecture:x64
```
Per forzare la terminazione di tutti i client, digitare:
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[utilizzando il comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
[utilizzando il comando /New-multicasttransmission](using-the-new-multicasttransmission-command.md)
[sottocomando: inizio MulticastTransmission](subcommand-start-multicasttransmission.md)
