---
title: Add-DriverGroupPackage
description: Argomento di riferimento per Add-DriverGroupPackage, che aggiunge un pacchetto driver a un gruppo di driver.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4baf4f16740e65c432cc09ca24270ab479346ac2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721107"
---
# <a name="add-drivergrouppackage"></a>Add-DriverGroupPackage

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge un pacchetto di driver a un gruppo di driver.

## <a name="syntax"></a>Sintassi
```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>Parametri

|         Parametro         |                                                                                                                                               Descrizione                                                                                                                                               |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DriverGroup<Group Name> |                                                                                                                                 Specifica il nome del gruppo di driver.                                                                                                                                 |
|   Server<Server name>   |                                                                                  Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.                                                                                  |
|   / DriverPackage:<Name>   |                                                                      Specifica il nome del pacchetto driver da aggiungere al gruppo. È necessario specificare questa opzione se il pacchetto driver non può essere identificato in modo univoco in base al nome.                                                                       |
|      PackageID<ID>      | Specifica l'ID per un pacchetto. Per trovare l'ID del pacchetto, fare clic sul gruppo di driver che contiene il pacchetto (o **tutti i pacchetti** nodo), fare doppio clic su pacchetto e quindi fare clic su **proprietà**. L'ID del pacchetto è elencato nella scheda **generale** , ad esempio: **{DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}**. |

## <a name="examples"></a>Esempi
Per aggiungere un pacchetto driver, digitare uno dei seguenti:
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
di sintassi della riga di comando usando il comando[Add-DriverGroupPackages](using-the-add-drivergrouppackages-command.md)
[usando il comando](using-the-add-driverpackage-command.md)
Add-driverpacke[usando il sottocomando Add-AllDriverPackages](using-the-add-alldriverpackages-subcommand.md)
