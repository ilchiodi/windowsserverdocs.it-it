---
title: Disable-TransportServer
description: Argomento di riferimento per Disable-TransportServer, che Disabilita tutti i servizi per un server di trasporto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81ae150b4f8e4de577e377a2d10a7a69675adac7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720961"
---
# <a name="disable-transportserver"></a>Disable-TransportServer

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disabilita tutti i servizi per un Server di trasporto.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del Server di trasporto deve essere disabilitata. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di Server di trasporto, verrà utilizzato il server locale.|
## <a name="examples"></a>Esempi
Per disabilitare il server, digitare:
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)

[utilizzando il comando Enable-TransportServer](using-the-enable-transportserver-command.md)
[utilizzando il comando Get-TransportServer](using-the-get-transportserver-command.md)sottocomando[: Set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: Start-TransportServer](subcommand-start-transportserver.md)
[sottocomando: Stop-TransportServer](subcommand-stop-transportserver.md)
