---
title: Utilizzando il comando Initialize-Server
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4a1f3a1c02eb21f630aaff7219610864cd1db2a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825192"
---
# <a name="using-the-initialize-server-command"></a>Utilizzando il comando Initialize-Server

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di configurare un server di servizi di distribuzione Windows per l'utilizzo iniziale dopo aver installato il ruolo del server. Dopo aver eseguito questo comando, è consigliabile usare la [utilizzando il comando add-immagine](using-the-add-image-command.md) comando per aggiungere immagini al server.
## <a name="syntax"></a>Sintassi
```
wdsutil /Initialize-Server [/Server:<Server name>] /remInst:<Full path> [/Authorize]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/remInst:"<Full path>"|Specifica il percorso completo e il nome della cartella remoteInstall. Se la cartella specificata non esiste già, questa opzione verrà creato quando viene eseguito il comando. È sempre necessario immettere un percorso locale, anche in caso di un computer remoto. Ad esempio:  **D:\remoteInstall**.|
|[/ Autorizzare]|Autorizza il server di controllo protocollo DHCP (Dynamic Host). Questa opzione è necessaria solo se è abilitato il rilevamento rogue DHCP, vale a dire che il PXE di servizi di distribuzione di Windows server deve essere autorizzato DHCP prima di eseguire la manutenzione computer client. Si noti che il rilevamento rogue DHCP è disabilitato per impostazione predefinita.|
## <a name="BKMK_examples"></a>Esempi
Per inizializzare il server e impostare la cartella condivisa remoteInstall nell'unità f:, digitare.
```
wdsutil /Initialize-Server /remInst:"F:\remoteInstall"
```
Per inizializzare il server e impostare la cartella condivisa remoteInstall nell'unità c:, digitare.
```
wdsutil /verbose /Progress /Initialize-Server /Server:MyWDSServer /remInst:"C:\remoteInstall"
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando disable-Server](using-the-disable-server-command.md)
[utilizzando il comando enable-Server](using-the-enable-server-command.md)
[tramite il Comando Get-Server](using-the-get-server-command.md)
[sottocomando: set-Server](subcommand-set-server.md)
[sottocomando: start-Server](subcommand-start-server.md)
[sottocomando: arresto-Server](subcommand-stop-server.md)
[il Server uninitialize opzione](the-uninitialize-server-option.md)
