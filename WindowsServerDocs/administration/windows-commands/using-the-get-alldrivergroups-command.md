---
title: Utilizzando il comando get-AllDriverGroups
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 236a2f798fb07ee6eafb9baf9314dbf46a984cdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874002"
---
# <a name="using-the-get-alldrivergroups-command"></a>Utilizzando il comando get-AllDriverGroups

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni su tutti i gruppi di driver in un server.
## <a name="syntax"></a>Sintassi
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|
|[/Show: {PackageMetaData &#124; filtri &#124; tutti i}]|Consente di visualizzare i metadati per tutti i pacchetti driver al gruppo specificato. **PackageMetaData** Visualizza informazioni su tutti i filtri per il gruppo di driver. **I filtri** consente di visualizzare i metadati per tutti i pacchetti driver e i filtri per il gruppo.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare informazioni su un file di driver, digitare:
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando get-DriverGroup](using-the-get-drivergroup-command.md)
