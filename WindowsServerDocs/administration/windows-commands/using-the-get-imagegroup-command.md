---
title: Get-ImageGroup
description: Windows Commands argomento per Get-ImageGroup, che recupera le informazioni su un gruppo di immagini e le immagini al suo interno.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0066e5d52c1d10b1f78ea627ee7a476bfd98f19d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830954"
---
# <a name="get-imagegroup"></a>Get-ImageGroup

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera le informazioni su un gruppo di immagini e le immagini al suo interno.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
mediaGroup:<Image group name>|Specifica il nome del gruppo di immagini.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|[/detailed]|Restituisce i metadati dell'immagine per ogni immagine. Se questo parametro non viene utilizzato, il comportamento predefinito consiste nel restituire solo il nome dell'immagine, la descrizione e il nome del file.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per visualizzare le informazioni su un gruppo di immagini, digitare:
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Per visualizzare le informazioni, inclusi i metadati, digitare:
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
usando il comando [add-ImageGroup](using-the-add-imagegroup-command.md)
[usando il comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)
[usando il comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
[sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)
