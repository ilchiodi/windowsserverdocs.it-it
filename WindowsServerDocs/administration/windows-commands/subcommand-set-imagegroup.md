---
title: Sottocomando set-ImageGroup
description: Argomento di riferimento per sottocomando set-ImageGroup, che modifica gli attributi di un gruppo di immagini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 429a8fee5b0236d264eb421f110219a1bc037368
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721703"
---
# <a name="subcommand-set-imagegroup"></a>Sottocomando: set-ImageGroup

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modificare gli attributi di un gruppo di immagini.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
MediaGroup<Image group name>|Specifica il nome del gruppo di immagini.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se non specificato, verrà utilizzato il server locale.|
|[/Name:<New image group name>]|Specifica il nuovo nome del gruppo di immagini.|
|[/ Sicurezza:<SDDL>]|Specifica il nuovo descrittore di protezione del gruppo di immagini, nel formato di sicurezza descriptor definition language (SDDL).|
## <a name="examples"></a>Esempi
Per impostare il nome per un gruppo di immagini, digitare:
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:New Image Group Name
```
Per specificare le impostazioni per un gruppo di immagini, digitare:
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:New Image Group Name 
/Security:O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando usando il comando[Add-ImageGroup](using-the-add-imagegroup-command.md)
[usando il comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)
[usando il comando Get-ImageGroup](using-the-get-imagegroup-command.md)
[con il comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
