---
title: Get-AllImageGroups
description: Argomento di riferimento per Get-AllImageGroups, che consente di recuperare informazioni su tutti i gruppi di immagini in un server e su tutte le immagini in tali gruppi di immagini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 204618955e91f1c9c9659d37ac3dfe2a01897c51
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720033"
---
# <a name="get-allimagegroups"></a>Get-AllImageGroups

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a>Esempi
Per visualizzare informazioni sui gruppi di immagine, digitare uno dei seguenti:
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
della sintassi della riga di comando usando il comando[Add-ImageGroup](using-the-add-imagegroup-command.md)
[usando il comando Get-ImageGroup](using-the-get-imagegroup-command.md)
[usando il comando Remove-ImageGroup](using-the-remove-imagegroup-command.md)
[sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)
