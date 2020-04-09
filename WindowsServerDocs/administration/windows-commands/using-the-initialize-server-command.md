---
title: Initialize-Server
description: Windows Commands argomento for Initialize-Server, che configura un server di servizi di distribuzione Windows per l'uso iniziale dopo l'installazione del ruolo del server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c80af07827c889a3dd1c5d3050cd2ca3b4c8f1e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830794"
---
# <a name="initialize-server"></a>Initialize-Server

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura un server di servizi di distribuzione Windows per l'utilizzo iniziale dopo l'installazione del ruolo del server. Dopo aver eseguito questo comando, usare il comando [Add-Image comando](using-the-add-image-command.md) per aggiungere immagini al server.
## <a name="syntax"></a>Sintassi
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/remInst:<Full path>|Specifica il percorso completo e il nome della cartella remoteInstall. Se la cartella specificata non esiste già, questa opzione la creerà quando viene eseguito il comando. È necessario immettere sempre un percorso locale, anche nel caso di un computer remoto. Ad esempio: **D:\remoteInstall**.|
|/Authorize|Autorizza il server in DHCP (Dynamic Host Control Protocol). Questa opzione è necessaria solo se è abilitata la funzionalità di rilevamento rogue DHCP, il che significa che il server PXE di servizi di distribuzione Windows deve essere autorizzato in DHCP prima di poter servire i computer client. Si noti che il rilevamento di Rogue DHCP è disabilitato per impostazione predefinita.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per inizializzare il server e impostare la cartella condivisa remoteInstall sull'unità F:, digitare.
```
wdsutil /Initialize-Server /remInst:F:\remoteInstall
```
Per inizializzare il server e impostare la cartella condivisa remoteInstall sull'unità C:, digitare.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:C:\remoteInstall
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[uso del comando disable-server](using-the-disable-server-command.md)
[uso del comando Enable-server](using-the-enable-server-command.md)
[usando il comando Get-server](using-the-get-server-command.md)
[sottocomando: set-server](subcommand-set-server.md)
[sottocomando: Start-Server](subcommand-start-server.md)
[sottocomando: Stop-](subcommand-stop-server.md) server
[l'opzione Uninitialize-server](the-uninitialize-server-option.md)
