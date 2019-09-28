---
title: Sottocomando stop-TransportServer
description: Argomento dei comandi di Windows per Stop-TransportServer
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc1b1eec-6893-445e-81dc-16b3fae287fa
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a2444328a426429c2dce5ceee3272cf1dc814cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370721"
---
# <a name="subcommand-stop-transportserver"></a>Sottocomando: stop-TransportServer

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arresta tutti i servizi in un Server di trasporto.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Stop-TransportServer [/Server:<Server name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del Server di trasporto. Può essere il nome NetBIOS oppure il nome di dominio completo. Se si specifica alcun Server di trasporto, verrà utilizzato il server locale.|
## <a name="BKMK_examples"></a>Esempi
Per arrestare i servizi, digitare uno dei seguenti:
```
wdsutil /Stop-TransportServer
wdsutil /verbose /Stop-TransportServer /Server:MyWDSServer
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando disable-TransportServer](using-the-disable-transportserver-command.md)
[utilizzando il comando enable-TransportServer](using-the-enable-transportserver-command.md)
[utilizzando il comando get-TransportServer](using-the-get-transportserver-command.md)
[sottocomando: set-TransportServer](subcommand-set-transportserver.md)
[sottocomando: start-TransportServer](subcommand-start-transportserver.md)
