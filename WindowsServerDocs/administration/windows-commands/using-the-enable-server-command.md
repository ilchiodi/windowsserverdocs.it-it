---
title: Enable-Server
description: Argomento di riferimento per Enable-Server, che Abilita tutti i servizi per servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf7bf57c0784fa16719b9f77da50212bca0ef850
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720926"
---
# <a name="enable-server"></a>Enable-Server

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Abilita tutti i servizi per servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="examples"></a>Esempi
Per abilitare i servizi nel server, eseguire una delle seguenti:
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando utilizzando il
[comando Disable-server](using-the-disable-server-command.md)
[utilizzando il comando Get-server](using-the-get-server-command.md)[utilizzando il comando Initialize-Server](using-the-initialize-server-command.md)
sottocomando[: set-server](subcommand-set-server.md)
[sottocomando: Start-Server](subcommand-start-server.md)
[sottocomando: stop-server](subcommand-stop-server.md)
[The Uninitialize-Server (opzione](the-uninitialize-server-option.md) )
