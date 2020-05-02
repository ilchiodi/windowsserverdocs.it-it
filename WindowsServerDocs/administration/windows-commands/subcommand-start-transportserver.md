---
title: Sottocomando Start-TransportServer
description: Argomento di riferimento per sottocomando Start-TransportServer, che avvia tutti i servizi per un server di trasporto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e93bc84-5b9e-4f9d-8cf0-1634417da0f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 92bd68421883c49ec29dfb78f06121bff880b01e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721641"
---
# <a name="subcommand-start-transportserver"></a>Sottocomando: start-TransportServer

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avvia tutti i servizi per un Server di trasporto.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /start-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del Server di trasporto. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="examples"></a>Esempi
Per avviare il server, digitare uno dei seguenti:
```
wdsutil /start-TransportServer
wdsutil /verbose /start-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)

utilizzando il[comando Disable-TransportServer](using-the-disable-transportserver-command.md)
[utilizzando il comando Enable-TransportServer](using-the-enable-transportserver-command.md)
[utilizzando il comando Get-TransportServer](using-the-get-transportserver-command.md)[sottocomando: Set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: Stop-TransportServer](subcommand-stop-transportserver.md)
