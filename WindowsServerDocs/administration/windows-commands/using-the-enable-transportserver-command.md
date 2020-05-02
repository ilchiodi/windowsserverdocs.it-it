---
title: Enable-TransportServer
description: Argomento di riferimento per Enable-TransportServer, che Abilita tutti i servizi per il server di trasporto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50cc381b1c178628be7d135868027b4f37787cdd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720924"
---
# <a name="enable-transportserver"></a>Enable-TransportServer

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Abilita tutti i servizi per il Server di trasporto.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del Server di trasporto. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome, verrà utilizzato il server locale.|
## <a name="examples"></a>Esempi
Per abilitare i servizi nel server, eseguire una delle seguenti:
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)

[utilizzando il comando Disable-TransportServer](using-the-disable-transportserver-command.md)
[utilizzando il comando Get-TransportServer](using-the-get-transportserver-command.md)sottocomando[: Set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: Start-TransportServer](subcommand-start-transportserver.md)
[sottocomando: Stop-TransportServer](subcommand-stop-transportserver.md)
