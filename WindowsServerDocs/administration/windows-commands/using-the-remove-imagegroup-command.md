---
title: Utilizzando il comando remove-ImageGroup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: df6af7fbd19cc95dfbffa0a5c0e2a4b3e33fb82c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877892"
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
mediaGroup:<Image group name>|Specifica il nome del gruppo di immagini da rimuovere|
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
