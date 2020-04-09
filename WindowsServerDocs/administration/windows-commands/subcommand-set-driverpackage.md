---
title: Set di sottocomandi-DriverPack
description: Windows Commands argomento for subcommand set-DriverPacking, che Rinomina e/o Abilita o Disabilita un pacchetto driver in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68d282b3338e67a33a55481658f55db69930b10e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834020"
---
# <a name="subcommand-set-driverpackage"></a>Sottocomando: set /InfFile

Rinomina e/o Abilita o disabilita un pacchetto di driver in un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

### <a name="parameters"></a>Parametri

|        Parametro         |                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<nome server >] |                                                                                                                                                 Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.                                                                                                                                                 |
| [/DriverPackage: nome\<>] |                                                                                                                                                                                       Specifica il nome corrente del pacchetto driver da modificare.                                                                                                                                                                                        |
|    [/PackageId: ID\<>]    | Specifica l'ID di servizi di distribuzione di Windows del pacchetto driver. È necessario specificare questa opzione se il pacchetto driver non può essere identificato in modo univoco in base al nome. Per trovare questo ID per un pacchetto, fare clic sul gruppo di driver che contiene il pacchetto (o **tutti i pacchetti** nodo), fare doppio clic su pacchetto e quindi fare clic su **proprietà**. L'ID del pacchetto è elencato nella scheda **generale** . Ad esempio: {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}. |
|   [/Name:\<nuovo nome >]    |                                                                                                                                                                                              Specifica il nuovo nome per il pacchetto driver.                                                                                                                                                                                              |
|      [/Enabled: {Sì      |                                                                                                                                                                                                                   Non                                                                                                                                                                                                                    |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per modificare le impostazioni relative a un pacchetto, digitare uno degli elementi seguenti:
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)