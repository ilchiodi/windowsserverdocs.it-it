---
title: Aggiungi-DriverPack
description: Windows Commands argomento per Add-DriverPack, che aggiunge un pacchetto driver al server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2ccdfcddd2f605eccd9cd32fed7b8c6921297fc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832014"
---
# <a name="add-driverpackage"></a>Aggiungi-DriverPack

Aggiunge un pacchetto driver al server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

### <a name="parameters"></a>Parametri

|          Parametro           |                                                              Descrizione                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|   InfFile:\<percorso file inf >   |                                           Specifica il percorso completo del file. inf da aggiungere.                                            |
|    /Server: nome del server\<>    | Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale. |
|      /Architettura: {x86      |                                                                 ia64                                                                  |
| [/DriverGroup: nome gruppo\<>] |                             Specifica il nome del gruppo di driver in cui deve essere aggiunto il pacchetto.                              |
|   [/Name: nome descrittivo\<>]   |                                           Indica il nome descrittivo per il pacchetto driver.                                            |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per aggiungere un pacchetto driver, digitare uno dei seguenti:
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:C:\Temp\Display.inf
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:C:\Temp\Display.inf /Architecture:x86 /DriverGroup:x86Drivers /Name:Display Driver
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

