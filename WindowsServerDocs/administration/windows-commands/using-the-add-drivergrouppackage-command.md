---
title: Add-DriverGroupPackage
description: Windows Commands argomento per Add-DriverGroupPackage, che aggiunge un pacchetto driver a un gruppo di driver.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cd323ae-9049-448e-a460-6c7d6462d4c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6753baeb03b99ee149250d41844469a5008f5ecd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832074"
---
# <a name="add-drivergrouppackage"></a>Add-DriverGroupPackage

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge un pacchetto di driver a un gruppo di driver.

## <a name="syntax"></a>Sintassi
```
wdsutil /add-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>Parametri

|         Parametro         |                                                                                                                                               Descrizione                                                                                                                                               |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:<Group Name> |                                                                                                                                 Specifica il nome del gruppo di driver.                                                                                                                                 |
|   /Server:<Server name>   |                                                                                  Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.                                                                                  |
|   /DriverPackage:<Name>   |                                                                      Specifica il nome del pacchetto driver da aggiungere al gruppo. È necessario specificare questa opzione se il pacchetto driver non può essere identificato in modo univoco in base al nome.                                                                       |
|      /PackageId:<ID>      | Specifica l'ID per un pacchetto. Per trovare l'ID del pacchetto, fare clic sul gruppo di driver che contiene il pacchetto (o **tutti i pacchetti** nodo), fare doppio clic su pacchetto e quindi fare clic su **proprietà**. L'ID del pacchetto è elencato nella scheda **generale** , ad esempio: **{DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}** . |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per aggiungere un pacchetto driver, digitare uno dei seguenti:
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /add-DriverGroupPackage /DriverGroup:printerdrivers /DriverPackage:XYZ
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando add-DriverGroupPackages](using-the-add-drivergrouppackages-command.md)
[utilizzando il comando Aggiungi /InfFile](using-the-add-driverpackage-command.md)
[utilizzando il sottocomando AllDriverPackages Aggiungi](using-the-add-alldriverpackages-subcommand.md)
