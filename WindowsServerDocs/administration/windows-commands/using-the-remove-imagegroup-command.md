---
title: Utilizzando il comando remove-ImageGroup
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51d9636013fef182c4abb74ae196e08c51ff11d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362751"
---
# <a name="using-the-remove-imagegroup-command"></a>Utilizzando il comando remove-ImageGroup

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove un gruppo di immagini da un server.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
mediaGroup: <Image group name>|Specifica il nome del gruppo di immagini da rimuovere|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="BKMK_examples"></a>Esempi
Per rimuovere il gruppo di immagini, digitare uno dei seguenti:
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:"My Image Group" /Server:MyWDSServer 
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando add-ImageGroup](using-the-add-imagegroup-command.md)
[utilizzando il comando get-AllImageGroups](using-the-get-allimagegroups-command.md)
[utilizzando il comando get-ImageGroup](using-the-get-imagegroup-command.md)
[sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)
