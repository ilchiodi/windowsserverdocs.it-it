---
title: Get-DriverGroup
description: Argomento di riferimento per Get-DriverGroup, che visualizza informazioni sui gruppi di driver in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cfe10c3-a63f-48e7-bef9-f6b474b4ddbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4804b699959b4fba2551e84379db97243f093ce7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719946"
---
# <a name="get-drivergroup"></a>Get-DriverGroup

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni sui gruppi di driver in un server.

## <a name="syntax"></a>Sintassi
```
wdsutil /Get-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|DriverGroup<Group Name>|Specifica il nome del gruppo di driver.|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN.  Se non si specifica un nome di server, viene utilizzato il server locale.|
|[/Show: {PackageMetaData &#124; filtri &#124; all}]|Consente di visualizzare i metadati per tutti i pacchetti driver del gruppo specificato. **PackageMetaData** Visualizza informazioni su tutti i filtri per il gruppo di driver. **Filtri** consente di visualizzare i metadati per tutti i pacchetti driver e i filtri per il gruppo.|
## <a name="examples"></a>Esempi
Per visualizzare le informazioni relative a un file di driver, digitare:
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Show:PackageMetaData
```
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Server:MyWdsServer /Show:Filters
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[con il comando Get-AllDriverGroups](using-the-get-alldrivergroups-command.md)
