---
title: Sottocomando Start-TransportServer
description: Argomento comandi di Windows per sottocomando Start-TransportServer, che avvia tutti i servizi per un server di trasporto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1356d415006324d75783d4e12ad6882d0fcc779
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833754"
---
# <a name="subcommand-start-transportserver"></a>Sottocomando: start-TransportServer

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avvia tutti i servizi per un Server di trasporto.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del Server di trasporto. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per avviare il server, digitare uno dei seguenti:
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando disable-TransportServer](using-the-disable-transportserver-command.md)
[utilizzando il comando enable-TransportServer](using-the-enable-transportserver-command.md)
[utilizzando il comando get-TransportServer](using-the-get-transportserver-command.md)
[sottocomando: set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: stop-TransportServer](subcommand-stop-transportserver.md)
