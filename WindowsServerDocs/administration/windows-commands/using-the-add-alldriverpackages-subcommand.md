---
title: Utilizzare il sottocomando Aggiungi AllDriverPackages
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f934d8c65da939fb60c564b375699f411b7c9ac
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440830"
---
# <a name="using-the-add-alldriverpackages-subcommand"></a>Utilizzare il sottocomando Aggiungi AllDriverPackages



Aggiunge tutti i pacchetti driver che vengono archiviati in una cartella a un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

## <a name="parameters"></a>Parametri

|          Parametro           |                                                              Descrizione                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  / FolderPath:\<percorso cartella >  |                      Specifica il percorso completo alla cartella che contiene i file. inf per i pacchetti driver.                      |
|   [/ Server:\<nome Server >]   | Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale. |
|     [/Architecture:{x86      |                                                                 ia64                                                                  |
| [/ DriverGroup:\<nome gruppo >] |                             Specifica il nome del gruppo di driver a cui devono essere aggiunti i pacchetti.                             |

## <a name="BKMK_examples"></a>Esempi

Per aggiungere pacchetti driver, digitare uno dei seguenti:
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers" /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers\Printers" /DriverGroup:"Printer Drivers"
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Add-WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)