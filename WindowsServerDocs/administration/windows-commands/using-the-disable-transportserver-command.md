---
title: Utilizzando il comando disable-TransportServer
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a009706b-8e89-486b-8e3d-512cd9f4de74
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d653aacf225333ac8a8b090ef53fb6826e84ab5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836982"
---
# <a name="using-the-disable-transportserver-command"></a>Utilizzando il comando disable-TransportServer

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Disabilita tutti i servizi per un Server di trasporto.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Disable-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del Server di trasporto deve essere disabilitata. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di Server di trasporto, verrà utilizzato il server locale.|
## <a name="BKMK_examples"></a>Esempi
Per disabilitare il server, digitare:
```
wdsutil /Disable-TransportServer
wdsutil /verbose /Disable-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando enable-TransportServer](using-the-enable-transportserver-command.md)
[utilizzando il comando get-TransportServer](using-the-get-transportserver-command.md)
[sottocomando: set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: start-TransportServer](subcommand-start-transportserver.md)
[sottocomando: stop-TransportServer](subcommand-stop-transportserver.md)
