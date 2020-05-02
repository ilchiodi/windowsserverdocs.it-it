---
title: Initialize-Server
description: Argomento di riferimento per Initialize-Server, che configura un server di servizi di distribuzione Windows per l'utilizzo iniziale dopo l'installazione del ruolo del server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54180923a077c0b423e73588bcbd1c03b0154d08
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719713"
---
# <a name="initialize-server"></a>Initialize-Server

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura un server di servizi di distribuzione Windows per l'utilizzo iniziale dopo l'installazione del ruolo del server. Dopo aver eseguito questo comando, usare il comando [Add-Image comando](using-the-add-image-command.md) per aggiungere immagini al server.
## <a name="syntax"></a>Sintassi
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|remInst<Full path>|Specifica il percorso completo e il nome della cartella remoteInstall. Se la cartella specificata non esiste già, questa opzione la creerà quando viene eseguito il comando. È necessario immettere sempre un percorso locale, anche nel caso di un computer remoto. Ad esempio: **D:\remoteInstall**.|
|/Authorize|Autorizza il server in DHCP (Dynamic Host Control Protocol). Questa opzione è necessaria solo se è abilitata la funzionalità di rilevamento rogue DHCP, il che significa che il server PXE di servizi di distribuzione Windows deve essere autorizzato in DHCP prima di poter servire i computer client. Si noti che il rilevamento di Rogue DHCP è disabilitato per impostazione predefinita.|
## <a name="examples"></a>Esempi
Per inizializzare il server e impostare la cartella condivisa remoteInstall sull'unità F:, digitare.
```
wdsutil /Initialize-Server /remInst:F:\remoteInstall
```
Per inizializzare il server e impostare la cartella condivisa remoteInstall sull'unità C:, digitare.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:C:\remoteInstall
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando utilizzando il
[comando Disable-server](using-the-disable-server-command.md)
[utilizzando il comando Enable-Server](using-the-enable-server-command.md)[utilizzando il comando Get-server](using-the-get-server-command.md)
sottocomando[: set-server](subcommand-set-server.md)
[sottocomando: Start-Server](subcommand-start-server.md)
[sottocomando: stop-server](subcommand-stop-server.md)
[l'opzione Uninitialize-server](the-uninitialize-server-option.md)
