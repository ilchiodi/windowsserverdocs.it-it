---
title: Get-AllDriverGroups
description: Argomento di riferimento per Get-AllDriverGroups, che consente di visualizzare informazioni su tutti i gruppi di driver in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f245ba53-f150-41b1-8418-38dcf0410a05
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4ac3e0c7b05c96383714c3a702cffd6aa8df18a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720895"
---
# <a name="get-alldrivergroups"></a>Get-AllDriverGroups

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni su tutti i gruppi di driver in un server.

## <a name="syntax"></a>Sintassi
```
wdsutil /Get-AllDriverGroups [/Server:<Server name>] [/Show:{PackageMetaData | Filters | All}]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|
|[/Show: {PackageMetaData &#124; filtri &#124; all}]|Consente di visualizzare i metadati per tutti i pacchetti driver del gruppo specificato. **PackageMetaData** Visualizza informazioni su tutti i filtri per il gruppo di driver. **Filtri** consente di visualizzare i metadati per tutti i pacchetti driver e i filtri per il gruppo.|
## <a name="examples"></a>Esempi
Per visualizzare le informazioni relative a un file di driver, digitare:
```
wdsutil /Get-AllDriverGroups /Server:MyWdsServer /Show:All
```
```
wdsutil /Get-AllDriverGroups [/Show:PackageMetaData]
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[con il comando Get-DriverGroup](using-the-get-drivergroup-command.md)
