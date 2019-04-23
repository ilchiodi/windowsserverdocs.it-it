---
title: Utilizzando il comando get-ImageGroup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9dcb76155dc1044730673ed46a53cad57441a246
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862602"
---
# <a name="using-the-get-imagegroup-command"></a>Utilizzando il comando get-ImageGroup

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informazioni su un gruppo di immagini e le immagini all'interno di esso.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
mediaGroup:<Image group name>|Specifica il nome del gruppo di immagini.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|[/ dettagliate]|Restituisce i metadati dell'immagine per ogni immagine. Se questo parametro non è in uso, il comportamento predefinito è per restituire solo il nome dell'immagine, descrizione e nome file.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare informazioni su un gruppo di immagini, digitare:
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Per visualizzare informazioni, inclusi metadati, digitare:
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[usando il comando add-ImageGroup](using-the-add-imagegroup-command.md)
[utilizzando il comando get-AllImageGroups](using-the-get-allimagegroups-command.md) 
 [ Utilizzando il comando remove-ImageGroup](using-the-remove-imagegroup-command.md)
[sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)
