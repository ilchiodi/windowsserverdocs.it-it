---
title: Avvio sottocomando-server
description: Windows Commands argomento per sottocomando Start-Server, che avvia tutti i servizi per un server di servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc3e0d11348dd6b531671e62139f2ce2c884c5ee
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833744"
---
# <a name="subcommand-start-server"></a>Sottocomando: start-Server

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Avvia tutti i servizi per un server di servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server deve essere avviato. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per avviare il server, digitare uno dei seguenti:
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando disable-Server](using-the-disable-server-command.md)
[utilizzando il comando enable-Server](using-the-enable-server-command.md)
[utilizzando il comando get-Server](using-the-get-server-command.md)
[utilizzando il comando di Server di inizializzazione](using-the-initialize-server-command.md)
[sottocomando: set Server](subcommand-set-server.md)
[sottocomando:-arresto del Server](subcommand-stop-server.md)
[il Server uninitialize (opzione)](the-uninitialize-server-option.md)
