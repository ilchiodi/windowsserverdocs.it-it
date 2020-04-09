---
title: Get-AllImageGroups
description: Windows Commands Topic for Get-AllImageGroups, che recupera informazioni su tutti i gruppi di immagini in un server e su tutte le immagini in tali gruppi di immagini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19285422612f8be34d39e6152fcf0300e1919b8a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831328"
---
# <a name="get-allimagegroups"></a>Get-AllImageGroups

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informazioni su tutti i gruppi di immagini in un server e tutte le immagini in tali gruppi di immagini.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|[/detailed]|Restituisce i metadati dell'immagine da ogni immagine. Se si omette questo parametro, il comportamento predefinito è per restituire solo il nome dell'immagine, descrizione e nome file per ogni immagine.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per visualizzare informazioni sui gruppi di immagine, digitare uno dei seguenti:
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando add-ImageGroup](using-the-add-imagegroup-command.md)
[utilizzando il comando get-ImageGroup](using-the-get-imagegroup-command.md)
[utilizzando il comando remove-ImageGroup](using-the-remove-imagegroup-command.md)
[sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)
