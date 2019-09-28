---
title: Utilizzando il comando Initialize-Server
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68a26ad9-5eb2-4490-b782-b7cd46b8000d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9e95972838fc70ee1e617d1e299c9e35db5b979
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392100"
---
# <a name="using-the-initialize-server-command"></a>Utilizzando il comando Initialize-Server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configura un server di servizi di distribuzione Windows per l'utilizzo iniziale dopo l'installazione del ruolo del server. Dopo aver eseguito questo comando, usare il comando [Add-Image comando](using-the-add-image-command.md) per aggiungere immagini al server.
## <a name="syntax"></a>Sintassi
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/remInst: "<Full path>"|Specifica il percorso completo e il nome della cartella remoteInstall. Se la cartella specificata non esiste già, questa opzione la creerà quando viene eseguito il comando. È necessario immettere sempre un percorso locale, anche nel caso di un computer remoto. Esempio: **D:\remoteInstall**.|
|/Authorize|Autorizza il server in DHCP (Dynamic Host Control Protocol). Questa opzione è necessaria solo se è abilitata la funzionalità di rilevamento rogue DHCP, il che significa che il server PXE di servizi di distribuzione Windows deve essere autorizzato in DHCP prima di poter servire i computer client. Si noti che il rilevamento di Rogue DHCP è disabilitato per impostazione predefinita.|
## <a name="BKMK_examples"></a>Esempi
Per inizializzare il server e impostare la cartella condivisa remoteInstall sull'unità F:, digitare.
```
wdsutil /Initialize-Server /remInst:"F:\remoteInstall"
```
Per inizializzare il server e impostare la cartella condivisa remoteInstall sull'unità C:, digitare.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:"C:\remoteInstall"
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[usando il comando disable-Server](using-the-disable-server-command.md)
[usando il comando Enable-Server](using-the-enable-server-command.md)
[usando il comando Get-server](using-the-get-server-command.md)
[sottocomando: set-server](subcommand-set-server.md)
[sottocomando: Start-Server](subcommand-start-server.md)1[sottocomando: stop-server](subcommand-stop-server.md)3[l'opzione Uninitialize-server](the-uninitialize-server-option.md)
