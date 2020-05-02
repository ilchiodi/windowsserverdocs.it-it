---
title: Get-ImageGroup
description: Argomento di riferimento per Get-ImageGroup, che recupera le informazioni su un gruppo di immagini e le immagini al suo interno.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30a87085cb935f95a209ffdd78ecf2b9fb45dc15
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719905"
---
# <a name="get-imagegroup"></a>Get-ImageGroup

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera le informazioni su un gruppo di immagini e le immagini al suo interno.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
MediaGroup<Image group name>|Specifica il nome del gruppo di immagini.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|[/detailed]|Restituisce i metadati dell'immagine per ogni immagine. Se questo parametro non viene utilizzato, il comportamento predefinito consiste nel restituire solo il nome dell'immagine, la descrizione e il nome del file.|
## <a name="examples"></a>Esempi
Per visualizzare le informazioni su un gruppo di immagini, digitare:
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Per visualizzare le informazioni, inclusi i metadati, digitare:
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando usando il comando[Add-ImageGroup](using-the-add-imagegroup-command.md)
[usando il comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)
[usando il comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
[sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)
