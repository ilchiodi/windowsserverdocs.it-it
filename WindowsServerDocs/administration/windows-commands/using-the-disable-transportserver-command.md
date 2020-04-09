---
title: Disable-TransportServer
description: Windows Commands argomento for disable-TransportServer, che Disabilita tutti i servizi per un server di trasporto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39f930a464364cda680098ef4e7e1081d0995503
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831624"
---
# <a name="disable-transportserver"></a>Disable-TransportServer

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disabilita tutti i servizi per un Server di trasporto.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del Server di trasporto deve essere disabilitata. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di Server di trasporto, verrà utilizzato il server locale.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per disabilitare il server, digitare:
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando enable-TransportServer](using-the-enable-transportserver-command.md)
[utilizzando il comando get-TransportServer](using-the-get-transportserver-command.md)
[sottocomando: set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: start-TransportServer](subcommand-start-transportserver.md)
[sottocomando: stop-TransportServer](subcommand-stop-transportserver.md)
