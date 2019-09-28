---
title: Utilizzando il comando get /InfFile
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f3d31d9a02454b0f7fca06b28a4df27174f7b02e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363194"
---
# <a name="using-the-get-driverpackage-command"></a>Utilizzando il comando get /InfFile



Visualizza informazioni su un pacchetto driver nel server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parametri

|        Parametro         |                                                                           Descrizione                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server: nome \<Server >] |              Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.               |
| [/DriverPackage: \<Name >] |                                                        Specifica il nome del pacchetto driver da visualizzare.                                                         |
|    [/PackageId: \<ID >]    | Specifica l'ID di servizi di distribuzione Windows del pacchetto driver da visualizzare. Se il pacchetto driver non può essere identificato in modo univoco in base al nome, è necessario specificare l'ID. |
|     [/Show: {Drivers     |                                                                              File                                                                               |

## <a name="BKMK_examples"></a>Esempi

Per visualizzare le informazioni relative a un pacchetto driver, digitare uno dei seguenti:
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)