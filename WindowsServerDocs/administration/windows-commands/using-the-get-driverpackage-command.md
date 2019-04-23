---
title: Utilizzando il comando get /InfFile
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6574177f1f0a8ead0cc2fa596380eb2c980f8d1f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888522"
---
# <a name="using-the-get-driverpackage-command"></a>Utilizzando il comando get /InfFile



Visualizza le informazioni relative a un pacchetto di driver sul server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Get-DriverPackage [/Server:<Server name>] {/DriverPackage:<Package Name> | /PackageId:<ID>} [/Show:{Drivers | Files | All}]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[/ Server:\<nome Server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|[/ Get /InfFile:\<nome >]|Specifica il nome del pacchetto driver da visualizzare.|
|[/PackageId:\<ID>]|Specifica l'ID di servizi di distribuzione Windows del pacchetto driver da visualizzare. Se il pacchetto driver non può essere identificato in modo univoco in base al nome, è necessario specificare l'ID.|
|[/Show: {driver | File | All}]|Indica le informazioni da visualizzare (se specificato). Se **/Mostra** non viene specificato, il valore predefinito è per restituire solo i driver dei metadati del pacchetto. **I driver** Visualizza tutti i driver nel pacchetto. **File** consente di visualizzare l'elenco dei file nel pacchetto. **Tutti i** consente di visualizzare i driver, file e metadati.|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare informazioni relative a un pacchetto driver, digitare uno dei seguenti:
```
WDSUTIL /Get-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Get-DriverPackage /DriverPackage:MyDriverPackage /Show:All
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)