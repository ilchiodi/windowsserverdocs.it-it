---
title: Rimuovi-DriverPack
description: Argomento dei comandi di Windows per Remove-DriverPack, che rimuove un pacchetto driver da un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b201e91-0d44-4e4a-8252-8b0235df1002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9eeebd0fd560f18aa49ac46f7eea30d8a9cc958
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830404"
---
# <a name="remove-driverpackage"></a>Rimuovi-DriverPack

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 

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

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per visualizzare informazioni sulle immagini, digitare uno dei seguenti:
```
wdsutil /remove-DriverPackage /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
```
wdsutil /remove-DriverPackage /Server:MyWdsServer /DriverPackage:MyDriverPackage
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando remove-DriverPackages](using-the-remove-driverpackages-command.md)
