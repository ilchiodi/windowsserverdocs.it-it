---
title: Il sottocomando set /InfFile
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 24acd672184b8df235e8de843961ac4adb2bd412
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441137"
---
# <a name="subcommand-set-driverpackage"></a>Sottocomando: set /InfFile



Rinomina e/o Abilita o disabilita un pacchetto di driver in un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

## <a name="parameters"></a>Parametri

|        Parametro         |                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/ Server:\<nome Server >] |                                                                                                                                                 Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.                                                                                                                                                 |
| [/ Get /InfFile:\<nome >] |                                                                                                                                                                                       Specifica il nome corrente del pacchetto driver da modificare.                                                                                                                                                                                        |
|    [/PackageId:\<ID>]    | Specifica l'ID di servizi di distribuzione di Windows del pacchetto driver. È necessario specificare questa opzione se il pacchetto driver non può essere identificato in modo univoco in base al nome. Per trovare questo ID per un pacchetto, fare clic sul gruppo di driver che contiene il pacchetto (o **tutti i pacchetti** nodo), fare doppio clic su pacchetto e quindi fare clic su **proprietà**. L'ID del pacchetto è elencato nel **Generale** scheda. Ad esempio: {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}. |
|   [/Name:\<nuovo nome >]    |                                                                                                                                                                                              Specifica il nuovo nome per il pacchetto driver.                                                                                                                                                                                              |
|      [O abilitati: {Sì      |                                                                                                                                                                                                                   No}                                                                                                                                                                                                                    |

## <a name="BKMK_examples"></a>Esempi

Per modificare le impostazioni relative a un pacchetto, digitare uno dei seguenti:
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)