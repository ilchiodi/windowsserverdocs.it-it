---
title: Add-ImageGroup
description: Windows Commands argomento per Add-ImageGroup, che aggiunge un gruppo di immagini a un server di servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f12ed27ca1a809ec34dbefbc4ff7288ff194a83e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831894"
---
# <a name="add-imagegroup"></a>Add-ImageGroup

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge un gruppo di immagini a un server di servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
mediaGroup:<Image group name>|Specifica il nome del gruppo di immagini da aggiungere.|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se non si specifica alcun nome server, verrà utilizzato il server locale.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per aggiungere un gruppo di immagini, digitare uno dei seguenti:
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando get-AllImageGroups](using-the-get-allimagegroups-command.md)
[utilizzando il comando get-ImageGroup](using-the-get-imagegroup-command.md)
[utilizzando il comando remove-ImageGroup](using-the-remove-imagegroup-command.md)
[sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)
