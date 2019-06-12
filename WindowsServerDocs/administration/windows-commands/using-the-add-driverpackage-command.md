---
title: Utilizzando il comando Aggiungi /InfFile
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 893bcd3b28aaa5d501017fe65b7b5205e9452693
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440694"
---
# <a name="using-the-add-driverpackage-command"></a>Utilizzando il comando Aggiungi /InfFile



Aggiunge un pacchetto driver al server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

## <a name="parameters"></a>Parametri

|          Parametro           |                                                              Descrizione                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|   InfFile:\<percorso del Inf File >   |                                           Specifica il percorso completo del file. inf da aggiungere.                                            |
|    / Server:\<nome Server >    | Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale. |
|      /Architecture:{x86      |                                                                 ia64                                                                  |
| [/ DriverGroup:\<nome gruppo >] |                             Specifica il nome del gruppo di driver in cui deve essere aggiunto il pacchetto.                              |
|   [/Name:\<nome descrittivo >]   |                                           Indica il nome descrittivo per il pacchetto driver.                                            |

## <a name="BKMK_examples"></a>Esempi

Per aggiungere un pacchetto driver, digitare uno dei seguenti:
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:"C:\Temp\Display.inf"
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:"C:\Temp\Display.inf" /Architecture:x86 /DriverGroup:x86Drivers /Name:"Display Driver"
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

