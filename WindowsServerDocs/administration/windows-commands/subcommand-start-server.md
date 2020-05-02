---
title: Avvio sottocomando-server
description: Argomento di riferimento per il sottocomando Start-Server, che avvia tutti i servizi per un server di servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2a6de007e62bf3be5544f97b53a4fcc13118985
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721646"
---
# <a name="subcommand-start-server"></a>Sottocomando: start-Server

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avvia tutti i servizi per un server di servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server deve essere avviato. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="examples"></a>Esempi
Per avviare il server, digitare uno dei seguenti:
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando utilizzando il comando[Disable-server](using-the-disable-server-command.md)
[utilizzando il comando Enable-Server](using-the-enable-server-command.md)
[utilizzando il comando](using-the-get-server-command.md)
get-server[utilizzando il comando Initialize-Server](using-the-initialize-server-command.md)
[sottocomando: set-server](subcommand-set-server.md)
[sottocomando: stop-server](subcommand-stop-server.md)
[l'opzione Uninitialize-server](the-uninitialize-server-option.md)
