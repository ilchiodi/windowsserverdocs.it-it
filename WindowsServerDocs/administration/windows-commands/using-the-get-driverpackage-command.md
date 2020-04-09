---
title: Get-DriverPack
description: Argomento comandi di Windows per Get-DriverPack, che consente di visualizzare informazioni su un pacchetto driver nel server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 94d231e4-ff01-48e7-9bc8-7b0d97a4339e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1906a109d22b24b5a44227d56c726996e6532bd6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831054"
---
# <a name="get-driverpackage"></a>Get-DriverPack

Visualizza informazioni su un pacchetto driver nel server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

### <a name="parameters"></a>Parametri

|        Parametro         |                                                                           Descrizione                                                                            |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<nome server >] |              Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.               |
| [/DriverPackage: nome\<>] |                                                        Specifica il nome del pacchetto driver da visualizzare.                                                         |
|    [/PackageId: ID\<>]    | Specifica l'ID di servizi di distribuzione Windows del pacchetto driver da visualizzare. Se il pacchetto driver non può essere identificato in modo univoco in base al nome, è necessario specificare l'ID. |
|     [/Show: {Drivers     |                                                                              Files                                                                               |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare le informazioni relative a un pacchetto driver, digitare uno dei seguenti:
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)