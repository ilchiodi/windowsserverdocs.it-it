---
title: Get-TransportServer
description: Windows Commands argomento per Get-TransportServer, che visualizza le informazioni su un server di trasporto specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 549eb32536bda201a81e5f40ef408359fae39f4b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830814"
---
# <a name="get-transportserver"></a>Get-TransportServer

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni su un Server di trasporto specificato.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/Show: {config}|Restituisce le informazioni di configurazione relative al server di trasporto specificato.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per visualizzare le informazioni sul server, digitare:
```
wdsutil /Get-TransportServer /Show:Config
```
Per visualizzare le informazioni di configurazione, digitare:
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[uso del comando disable-TransportServer](using-the-disable-transportserver-command.md)
[uso del comando Enable-TransportServer](using-the-enable-transportserver-command.md)
sottocomando [: Set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: Start-TransportServer](subcommand-start-transportserver.md)
[sottocomando: Stop-TransportServer](subcommand-stop-transportserver.md)
