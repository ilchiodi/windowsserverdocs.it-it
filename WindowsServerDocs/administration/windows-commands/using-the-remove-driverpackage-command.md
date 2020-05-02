---
title: Rimuovi-DriverPack
description: Argomento di riferimento per Remove-DriverPack, che rimuove un pacchetto driver da un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b201e91-0d44-4e4a-8252-8b0235df1002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 623fa7bb22c4aa4e545156cf0b214a4042fb90a3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720376"
---
# <a name="remove-driverpackage"></a>Rimuovi-DriverPack

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 

Rimuove un pacchetto driver da un server.

## <a name="syntax"></a>Sintassi
```
wdsutil /remove-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
### <a name="parameters"></a>Parametri

|        Parametro        |                                                                            Descrizione                                                                             |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |              Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.              |
| [/ DriverPackage:<Name>] |                                                        Specifica il nome del pacchetto driver da rimuovere.                                                         |
|    [/ PackageId:<ID>]    | Specifica l'ID di servizi di distribuzione Windows del pacchetto driver da rimuovere. Se il pacchetto driver non può essere identificato in modo univoco in base al nome, è necessario specificare l'ID. |

## <a name="examples"></a>Esempi
Per visualizzare informazioni sulle immagini, digitare uno dei seguenti:
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
[con il comando Remove-DriverPackages](using-the-remove-driverpackages-command.md)
