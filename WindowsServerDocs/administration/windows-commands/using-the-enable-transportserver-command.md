---
title: Utilizzando il comando enable-TransportServer
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d79dba1-4b57-4a00-8cba-877e6b8618e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 40793ac0b9dc7d8b4a80d6a66b55244202aa37d1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834332"
---
# <a name="using-the-enable-transportserver-command"></a>Utilizzando il comando enable-TransportServer

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Abilita tutti i servizi per il Server di trasporto.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Enable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del Server di trasporto. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome, verrà utilizzato il server locale.|
## <a name="BKMK_examples"></a>Esempi
Per abilitare i servizi nel server, eseguire una delle seguenti:
```
wdsutil /Enable-TransportServer
wdsutil /verbose /Enable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando disable-TransportServer](using-the-disable-transportserver-command.md)
[utilizzando il comando get-TransportServer](using-the-get-transportserver-command.md)
[sottocomando: set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: start-TransportServer](subcommand-start-transportserver.md)
[sottocomando: stop-TransportServer](subcommand-stop-transportserver.md)
