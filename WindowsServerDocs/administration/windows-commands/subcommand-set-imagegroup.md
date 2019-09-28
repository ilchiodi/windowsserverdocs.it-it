---
title: Sottocomando set-ImageGroup
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10023e493ae4db51783b7401c12bc1605145b86c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370795"
---
# <a name="subcommand-set-imagegroup"></a>Sottocomando: set-ImageGroup

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica gli attributi di un gruppo di immagini.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
mediaGroup: <Image group name>|Specifica il nome del gruppo di immagini.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se non specificato, verrà utilizzato il server locale.|
|[/Name:<New image group name>]|Specifica il nuovo nome del gruppo di immagini.|
|[/ Sicurezza:<SDDL>]|Specifica il nuovo descrittore di protezione del gruppo di immagini, nel formato di sicurezza descriptor definition language (SDDL).|
## <a name="BKMK_examples"></a>Esempi
Per impostare il nome per un gruppo di immagini, digitare:
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:"New Image Group Name"
```
Per specificare le impostazioni per un gruppo di immagini, digitare:
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:"New Image Group Name" 
/Security:"O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)"
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando add-ImageGroup](using-the-add-imagegroup-command.md)
[utilizzando il comando get-AllImageGroups](using-the-get-allimagegroups-command.md)
[utilizzando il comando get-ImageGroup](using-the-get-imagegroup-command.md)
[utilizzando il comando remove-ImageGroup](using-the-remove-imagegroup-command.md)
