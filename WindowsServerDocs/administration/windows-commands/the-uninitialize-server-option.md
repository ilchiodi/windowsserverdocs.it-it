---
title: Uninitialize-server
description: Argomento di riferimento per Uninitialize-server, che ripristina le modifiche apportate al server durante la configurazione iniziale del server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d7747a44172b7382bd22a7d48ccc717a89ccacb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721390"
---
# <a name="uninitialize-server"></a>Uninitialize-server

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ripristina le modifiche apportate al server durante la configurazione iniziale del server. Sono incluse le modifiche apportate dall'opzione **/Initialize-Server** o dallo snap-in MMC Servizi di distribuzione Windows. Si noti che questo comando Reimposta il server a uno stato non configurato. Questo comando non modifica il contenuto della cartella condivisa remoteInstall. Piuttosto, Ripristina lo stato del server in modo che è possibile reinizializzare il server.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="examples"></a>Esempi
Per reinizializzare il server, digitare uno dei seguenti:
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando utilizzando il
[comando Disable-server](using-the-disable-server-command.md)[utilizzando il comando Enable-Server](using-the-enable-server-command.md)
[utilizzando il comando](using-the-get-server-command.md)
get-server[utilizzando il comando Initialize-Server](using-the-initialize-server-command.md)
sottocomando[: set-server](subcommand-set-server.md)
[sottocomando: Start-Server](subcommand-start-server.md)
[sottocomando: stop-server](subcommand-stop-server.md)
