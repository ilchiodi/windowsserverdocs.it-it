---
title: Utilizzando il comando get-AllImageGroups
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54e302dca5014d084c7277154eb491f9e33a536b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363308"
---
# <a name="using-the-get-allimagegroups-command"></a>Utilizzando il comando get-AllImageGroups

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informazioni su tutti i gruppi di immagini in un server e tutte le immagini in tali gruppi di immagini.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|[/detailed]|Restituisce i metadati dell'immagine da ogni immagine. Se si omette questo parametro, il comportamento predefinito è per restituire solo il nome dell'immagine, descrizione e nome file per ogni immagine.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare informazioni sui gruppi di immagine, digitare uno dei seguenti:
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando add-ImageGroup](using-the-add-imagegroup-command.md)
[utilizzando il comando get-ImageGroup](using-the-get-imagegroup-command.md)
[utilizzando il comando remove-ImageGroup](using-the-remove-imagegroup-command.md)
[sottocomando: set-ImageGroup](subcommand-set-imagegroup.md)
