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
ms.openlocfilehash: 30e4c2c7c5cf2668d62e96d8d2a54dc33e3d2a55
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960948"
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
#### <a name="additional-references"></a>riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)  
[Uso del comando Add-ImageGroup](using-the-add-imagegroup-command.md)  
[Utilizzando il comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)  
[Utilizzando il comando Get-ImageGroup](using-the-get-imagegroup-command.md)  
[Sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)  
