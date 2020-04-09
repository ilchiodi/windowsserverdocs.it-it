---
title: Enable-TransportServer
description: Windows Commands argomento per Enable-TransportServer, che Abilita tutti i servizi per il server di trasporto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ee5f1206633fb47c4a4b066c099fbf9c7020fb6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831484"
---
# <a name="enable-transportserver"></a>Enable-TransportServer

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Abilita tutti i servizi per il Server di trasporto.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del Server di trasporto. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome, verrà utilizzato il server locale.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per abilitare i servizi nel server, eseguire una delle seguenti:
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando disable-TransportServer](using-the-disable-transportserver-command.md)
[utilizzando il comando get-TransportServer](using-the-get-transportserver-command.md)
[sottocomando: set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: start-TransportServer](subcommand-start-transportserver.md)
[sottocomando: stop-TransportServer](subcommand-stop-transportserver.md)
