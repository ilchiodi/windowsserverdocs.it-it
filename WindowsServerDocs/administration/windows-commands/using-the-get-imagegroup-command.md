---
title: Utilizzando il comando get-ImageGroup
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6531a3b69840a0a4910b2effdd3e349b76edf2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363124"
---
# <a name="using-the-get-imagegroup-command"></a>Utilizzando il comando get-ImageGroup

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera le informazioni su un gruppo di immagini e le immagini al suo interno.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
mediaGroup: <Image group name>|Specifica il nome del gruppo di immagini.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|[/detailed]|Restituisce i metadati dell'immagine per ogni immagine. Se questo parametro non viene utilizzato, il comportamento predefinito consiste nel restituire solo il nome dell'immagine, la descrizione e il nome del file.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare le informazioni su un gruppo di immagini, digitare:
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Per visualizzare le informazioni, inclusi i metadati, digitare:
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[usando il comando add-ImageGroup](using-the-add-imagegroup-command.md)
[usando il comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)
[usando il comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
[sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)
