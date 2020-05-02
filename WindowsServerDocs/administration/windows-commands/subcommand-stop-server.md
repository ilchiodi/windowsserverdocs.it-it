---
title: Arresto sottocomando-server
description: Argomento di riferimento per sottocomando stop-server, che arresta tutti i servizi in un server di servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68cc73ac016e2ffded774567034801e1c11944d1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721626"
---
# <a name="subcommand-stop-server"></a>Sottocomando: arresto-Server

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arresta tutti i servizi in un server di servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="examples"></a>Esempi
Per arrestare i servizi, digitare uno dei seguenti:
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando utilizzando il comando[Disable-server](using-the-disable-server-command.md)
[utilizzando il comando Enable-Server](using-the-enable-server-command.md)
[utilizzando il comando](using-the-get-server-command.md)
get-server[utilizzando il comando Initialize-Server](using-the-initialize-server-command.md)
[sottocomando: set-server](subcommand-set-server.md)
[sottocomando: Start-Server](subcommand-start-server.md)
[l'opzione Uninitialize-server](the-uninitialize-server-option.md)
