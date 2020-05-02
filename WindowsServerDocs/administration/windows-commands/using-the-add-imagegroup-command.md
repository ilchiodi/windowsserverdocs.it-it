---
title: Add-ImageGroup
description: Argomento di riferimento per Add-ImageGroup, che aggiunge un gruppo di immagini a un server di servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b08042ac6b33c0ccfe0b66bb0fec70805d55d75f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721047"
---
# <a name="add-imagegroup"></a>Add-ImageGroup

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge un gruppo di immagini a un server di servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
MediaGroup<Image group name>|Specifica il nome del gruppo di immagini da aggiungere.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se non si specifica alcun nome server, verrà utilizzato il server locale.|
## <a name="examples"></a>Esempi
Per aggiungere un gruppo di immagini, digitare uno dei seguenti:
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando[tramite il comando](using-the-get-allimagegroups-command.md)
Get-AllImageGroups[utilizzando il comando Get-ImageGroup](using-the-get-imagegroup-command.md)
[utilizzando il comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
[sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)
