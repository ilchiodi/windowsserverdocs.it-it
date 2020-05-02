---
title: Remove-ImageGroup
description: Argomento di riferimento per Remove-ImageGroup, che rimuove un gruppo di immagini da un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f814d83a32a8c739e7462bc77251cf3f3f4fe20e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720347"
---
# <a name="using-the-remove-imagegroup-command"></a>Utilizzando il comando remove-ImageGroup

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove un gruppo di immagini da un server.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
MediaGroup<Image group name>|Specifica il nome del gruppo di immagini da rimuovere|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
## <a name="examples"></a>Esempi
Per rimuovere il gruppo di immagini, digitare uno dei seguenti:
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:My Image Group /Server:MyWDSServer 
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
[Utilizzando il comando add-ImageGroup](using-the-add-imagegroup-command.md)  
[Utilizzando il comando get-AllImageGroups](using-the-get-allimagegroups-command.md)  
[Utilizzando il comando get-ImageGroup](using-the-get-imagegroup-command.md)  
[Sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)  
