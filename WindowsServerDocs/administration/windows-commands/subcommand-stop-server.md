---
title: 'Sottocomando: arresto del Server'
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09f411c0-099f-4591-95fd-b77b3fd9118a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddb681234cfcbe6d02e56f2e366167faeeb25280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834732"
---
# <a name="subcommand-stop-server"></a>Sottocomando: arresto-Server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arresta tutti i servizi in un server di servizi di distribuzione Windows.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Stop-Server [/Server:<Server name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="BKMK_examples"></a>Esempi
Per arrestare i servizi, digitare uno dei seguenti:
```
wdsutil /Stop-Server
wdsutil /verbose /Stop-Server /Server:MyWDSServer
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando disable-Server](using-the-disable-server-command.md)
[utilizzando il comando enable-Server](using-the-enable-server-command.md)
[utilizzando il comando get-Server](using-the-get-server-command.md)
[utilizzando il comando di Server di inizializzazione](using-the-initialize-server-command.md)
[sottocomando: set Server](subcommand-set-server.md)
[sottocomando: avviare Server](subcommand-start-server.md)
[il Server uninitialize (opzione)](the-uninitialize-server-option.md)
