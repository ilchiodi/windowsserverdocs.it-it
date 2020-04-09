---
title: Uninitialize-server
description: Windows Commands argomento for Uninitialize-server, che ripristina le modifiche apportate al server durante la configurazione iniziale del server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ed912af1ec3bc505e1e7465650a119af979b407
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833114"
---
# <a name="uninitialize-server"></a>Uninitialize-server

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ripristina le modifiche apportate al server durante la configurazione iniziale del server. Sono incluse le modifiche apportate dall'opzione **/Initialize-Server** o dallo snap-in MMC Servizi di distribuzione Windows. Si noti che questo comando Reimposta il server a uno stato non configurato. Questo comando non modifica il contenuto della cartella condivisa remoteInstall. Piuttosto, Ripristina lo stato del server in modo che è possibile reinizializzare il server.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per reinizializzare il server, digitare uno dei seguenti:
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando disable-Server](using-the-disable-server-command.md)
[utilizzando il comando enable-Server](using-the-enable-server-command.md)
[utilizzando il comando get-Server](using-the-get-server-command.md)
[utilizzando il comando di Server di inizializzazione](using-the-initialize-server-command.md)
[sottocomando: set Server](subcommand-set-server.md)
[sottocomando: avviare Server](subcommand-start-server.md)
[sottocomando:-arresto del Server](subcommand-stop-server.md)
