---
title: Utilizzando il comando get /InfFile
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0f123d281625140b3c4ba46316cb9b773bf5fee
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440511"
---
# <a name="using-the-get-driverpackage-command"></a>Utilizzando il comando get /InfFile



Visualizza le informazioni relative a un pacchetto di driver sul server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parametri

|        Parametro         |                                                                           Descrizione                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/ Server:\<nome Server >] |              Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.               |
| [/ Get /InfFile:\<nome >] |                                                        Specifica il nome del pacchetto driver da visualizzare.                                                         |
|    [/PackageId:\<ID>]    | Specifica l'ID di servizi di distribuzione Windows del pacchetto driver da visualizzare. Se il pacchetto driver non può essere identificato in modo univoco in base al nome, è necessario specificare l'ID. |
|     [/Show: {driver     |                                                                              File                                                                               |

## <a name="BKMK_examples"></a>Esempi

Per visualizzare informazioni relative a un pacchetto driver, digitare uno dei seguenti:
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)