---
title: Utilizzando il comando remove-DriverGroupPackage
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e48616d-d6a4-45f0-a5c6-efe62bf6a0ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82a8fb8fbe9e713c3e22c08839bc4bc22fe900db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883992"
---
# <a name="using-the-remove-drivergrouppackage-command"></a>Utilizzando il comando remove-DriverGroupPackage



Rimuove un pacchetto driver da un gruppo di driver in un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[/ Server:\<nome Server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|
|[/ Get /InfFile:\<nome >]|Specifica il nome del pacchetto driver da rimuovere.|
|[/PackageId:\<ID>]|Specifica l'ID di servizi di distribuzione Windows del pacchetto driver da rimuovere. È necessario specificare questa opzione se il pacchetto driver non può essere identificato in modo univoco in base al nome.|

## <a name="BKMK_examples"></a>Esempi

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /DriverPackage:XYZ
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)