---
title: Utilizzando il comando remove /InfFile
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b201e91-0d44-4e4a-8252-8b0235df1002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 002af67ab721d308cfc6421b37a089536ab61862
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837212"
---
# <a name="using-the-remove-driverpackage-command"></a>Utilizzando il comando remove /InfFile

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove un pacchetto driver da un server.
## <a name="syntax"></a>Sintassi
```
wdsutil /remove-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|
|[/ DriverPackage:<Name>]|Specifica il nome del pacchetto driver da rimuovere.|
|[/ PackageId:<ID>]|Specifica l'ID di servizi di distribuzione Windows del pacchetto driver da rimuovere. Se il pacchetto driver non può essere identificato in modo univoco in base al nome, è necessario specificare l'ID.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare informazioni sulle immagini, digitare uno dei seguenti:
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando remove-DriverPackages](using-the-remove-driverpackages-command.md)
