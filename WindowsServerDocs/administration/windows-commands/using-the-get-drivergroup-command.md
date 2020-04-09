---
title: Get-DriverGroup
description: Windows Commands argomento per Get-DriverGroup, che visualizza informazioni sui gruppi di driver in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cfe10c3-a63f-48e7-bef9-f6b474b4ddbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60a0649e98a0af0cbbd12db9a07f97dade66344c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831094"
---
# <a name="get-drivergroup"></a>Get-DriverGroup

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni sui gruppi di driver in un server.

## <a name="syntax"></a>Sintassi
```
wdsutil /Get-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/DriverGroup:<Group Name>|Specifica il nome del gruppo di driver.|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN.  Se non si specifica un nome di server, viene utilizzato il server locale.|
|[/Show: {PackageMetaData &#124; filters &#124; all}]|Consente di visualizzare i metadati per tutti i pacchetti driver del gruppo specificato. **PackageMetaData** Visualizza informazioni su tutti i filtri per il gruppo di driver. **Filtri** consente di visualizzare i metadati per tutti i pacchetti driver e i filtri per il gruppo.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per visualizzare le informazioni relative a un file di driver, digitare:
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Show:PackageMetaData
```
```
wdsutil /Get-DriverGroup /DriverGroup:printerdrivers /Server:MyWdsServer /Show:Filters
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- 
della [chiave della sintassi della riga di comando](command-line-syntax-key.md) [tramite il comando Get-AllDriverGroups](using-the-get-alldrivergroups-command.md)
