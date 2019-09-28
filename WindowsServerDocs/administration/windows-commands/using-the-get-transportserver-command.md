---
title: Utilizzando il comando get-TransportServer
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de634123-0179-41b2-9c6f-726508130ff5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 282b69162cf3550c5bcba3282b60f15072c96ed6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363067"
---
# <a name="using-the-get-transportserver-command"></a>Utilizzando il comando get-TransportServer

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni su un Server di trasporto specificato.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-TransportServer [/Server:<Server name>] /Show:{Config}
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/Show: {config}|Restituisce le informazioni di configurazione relative al server di trasporto specificato.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare le informazioni sul server, digitare:
```
wdsutil /Get-TransportServer /Show:Config
```
Per visualizzare le informazioni di configurazione, digitare:
```
wdsutil /Get-TransportServer /Server:MyWDSServer /Show:Config
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[usando il comando disable-TransportServer](using-the-disable-transportserver-command.md)
[usando il comando Enable-TransportServer](using-the-enable-transportserver-command.md)
 sottocomando[: Set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: Start-TransportServer](subcommand-start-transportserver.md)
[sottocomando: Stop-TransportServer](subcommand-stop-transportserver.md)
