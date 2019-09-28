---
title: Il Server uninitialize (opzione)
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 015efb04-fe84-469f-bd81-49d0046296b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c63e09738871c5b74c1b564a83c35ad28f4fa80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385587"
---
# <a name="the-uninitialize-server-option"></a>Il Server uninitialize (opzione)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ripristina le modifiche apportate al server durante la configurazione iniziale del server. Sono incluse le modifiche apportate dall'opzione **/Initialize-Server** o dallo snap-in MMC Servizi di distribuzione Windows. Si noti che questo comando Reimposta il server a uno stato non configurato. Questo comando non modifica il contenuto della cartella condivisa remoteInstall. Piuttosto, Ripristina lo stato del server in modo che è possibile reinizializzare il server.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Uninitialize-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="BKMK_examples"></a>Esempi
Per reinizializzare il server, digitare uno dei seguenti:
```
wdsutil /Uninitialize-Server
wdsutil /verbose /Uninitialize-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando disable-Server](using-the-disable-server-command.md)
[utilizzando il comando enable-Server](using-the-enable-server-command.md)
[utilizzando il comando get-Server](using-the-get-server-command.md)
[utilizzando il comando di Server di inizializzazione](using-the-initialize-server-command.md)
[sottocomando: set Server](subcommand-set-server.md)
[sottocomando: avviare Server](subcommand-start-server.md)
[sottocomando:-arresto del Server](subcommand-stop-server.md)
