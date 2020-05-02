---
title: Sottocomando stop-TransportServer
description: Argomento di riferimento per Stop-TransportServer
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4321ec991b2c20911f992e4c3c38e5c9cfa5f165
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721616"
---
# <a name="subcommand-stop-transportserver"></a>Sottocomando: stop-TransportServer

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arresta tutti i servizi in un Server di trasporto.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del Server di trasporto. Può essere il nome NetBIOS oppure il nome di dominio completo. Se si specifica alcun Server di trasporto, verrà utilizzato il server locale.|
## <a name="examples"></a><a name="BKMK_examples"></a>Esempi
Per arrestare i servizi, digitare uno dei seguenti:
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
utilizzando il
[comando Disable-TransportServer](using-the-disable-transportserver-command.md)[utilizzando il comando](using-the-enable-transportserver-command.md)
Enable-TransportServer[utilizzando il comando Get-TransportServer](using-the-get-transportserver-command.md)
[sottocomando: Set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: Start-TransportServer](subcommand-start-transportserver.md)
