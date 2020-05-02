---
title: Utilizzare il sottocomando Aggiungi AllDriverPackages
description: Argomento di riferimento per Add-AllDriverPackages, che aggiunge tutti i pacchetti driver archiviati in una cartella a un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 31daa8fc3e3304dba5079672ea4619fd085dd74f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721163"
---
# <a name="add-alldriverpackages"></a>Add-AllDriverPackages

Aggiunge tutti i pacchetti driver che vengono archiviati in una cartella a un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

### <a name="parameters"></a>Parametri

|          Parametro           |                                                              Descrizione                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  /FolderPath:\<percorso cartella>  |                      Specifica il percorso completo alla cartella che contiene i file. inf per i pacchetti driver.                      |
|   [/Server:\<nome server>]   | Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale. |
|     [/Architettura: {x86      |                                                                 ia64                                                                  |
| [/DriverGroup:\<nome gruppo>] |                             Specifica il nome del gruppo di driver a cui devono essere aggiunti i pacchetti.                             |

## <a name="examples"></a>Esempi

Per aggiungere pacchetti driver, digitare uno dei seguenti:
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:C:\Temp\Drivers\Printers /DriverGroup:Printer Drivers
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Add-WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)