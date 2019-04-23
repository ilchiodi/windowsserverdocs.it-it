---
title: Utilizzando il comando get-DriverGroup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cfe10c3-a63f-48e7-bef9-f6b474b4ddbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f82969e03b3474cf39afd2ae5c3ef2f9d4f8b5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847152"
---
# <a name="using-the-get-drivergroup-command"></a>Utilizzando il comando get-DriverGroup

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni sui gruppi di driver in un server.
## <a name="syntax"></a>Sintassi
```
wdsutil /Get-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/DriverGroup:<Group Name>|Specifica il nome del gruppo di driver.|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN.  Se non viene specificato un nome di server, viene utilizzato il server locale.|
|[/Show: {PackageMetaData &#124; filtri &#124; tutti i}]|Consente di visualizzare i metadati per tutti i pacchetti driver al gruppo specificato. **PackageMetaData** Visualizza informazioni su tutti i filtri per il gruppo di driver. **I filtri** consente di visualizzare i metadati per tutti i pacchetti driver e i filtri per il gruppo.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare informazioni su un file di driver, digitare:
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Show:PackageMetaData
```
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Server:MyWdsServer /Show:Filters
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[usando il comando get-AllDriverGroups](using-the-get-alldrivergroups-command.md)
