---
title: Set di sottocomandi-DriverPack
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 65751cf6e03baa87c7734b318a26111652bee0a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370817"
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
| [/Server: nome \<Server >] |                                                                                                                                                 Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.                                                                                                                                                 |
| [/DriverPackage: \<Name >] |                                                                                                                                                                                       Specifica il nome corrente del pacchetto driver da modificare.                                                                                                                                                                                        |
|    [/PackageId: \<ID >]    | Specifica l'ID di servizi di distribuzione di Windows del pacchetto driver. È necessario specificare questa opzione se il pacchetto driver non può essere identificato in modo univoco in base al nome. Per trovare questo ID per un pacchetto, fare clic sul gruppo di driver che contiene il pacchetto (o **tutti i pacchetti** nodo), fare doppio clic su pacchetto e quindi fare clic su **proprietà**. L'ID del pacchetto è elencato nel **Generale** scheda. Ad esempio: {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}. |
|   [/Name: \<New nome >]    |                                                                                                                                                                                              Specifica il nuovo nome per il pacchetto driver.                                                                                                                                                                                              |
|      [/Enabled: {Sì      |                                                                                                                                                                                                                   non                                                                                                                                                                                                                    |

## <a name="BKMK_examples"></a>Esempi

Per modificare le impostazioni relative a un pacchetto, digitare uno degli elementi seguenti:
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)