---
title: Utilizzando il comando add-ImageDriverPackage
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c2a4833-6427-47f8-9ffb-20b3786cb406
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1069b7c63e8b3bbc28fd900e8c869afc8abc03bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363731"
---
# <a name="using-the-add-imagedriverpackage-command"></a>Utilizzando il comando add-ImageDriverPackage

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

aggiunge un pacchetto driver presente nell'archivio driver a un'immagine di avvio esistente sul server. La versione di immagine deve essere Windows 7 o Windows Server 2008 R2 o versioni successive.
## <a name="syntax"></a>Sintassi
```
wdsutil /add-ImageDriverPackage [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
```
```
[/Filename:<File name>] {/DriverPackage:<Package Name> | /PackageId:<ID>}
```
## <a name="parameters"></a>Parametri

|                 Parametro                  |                                                                                                                                                                                                            Descrizione                                                                                                                                                                                                             |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           [/Server:<Server name>           |                                                                                                                                               Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.                                                                                                                                                |
|             supporto:<Image name>             |                                                                                                                                                                                       Specifica il nome dell'immagine da aggiungere il driver.                                                                                                                                                                                        |
|               mediatype:Boot               |                                                                                                                                                                Specifica il tipo di immagine per aggiungere il driver. Pacchetti driver possono essere aggiunti solo alle immagini di avvio.                                                                                                                                                                 |
| / Architettura: {x86 &#124; ia64 &#124; x64} |                                                                                                       Specifica l'architettura dell'immagine di avvio. Poiché è possibile che lo stesso nome di immagine per immagini di avvio diverse architetture, è necessario specificare l'architettura per garantire che venga utilizzata l'immagine corretta.                                                                                                        |
|           / Filename:<File name>]           |                                                                                                                                                        Specifica il nome del file. Se l'immagine non può essere identificata in modo univoco dal nome, è necessario specificare il nome del file.                                                                                                                                                        |
|           [/DriverPackage:<Name>           |                                                                                                                                                                                   Specifica il nome del pacchetto driver da aggiungere all'immagine.                                                                                                                                                                                    |
|             [/ PackageId:<ID>]              | Specifica l'ID di servizi di distribuzione di Windows del pacchetto driver. È necessario specificare questa opzione se il pacchetto driver non può essere identificato in modo univoco in base al nome. Per trovare l'ID del pacchetto, fare clic sul gruppo di driver che contiene il pacchetto (o **tutti i pacchetti** nodo), fare doppio clic su pacchetto e quindi fare clic su **proprietà**. L'ID del pacchetto è elencato nella scheda **generale** . Ad esempio: {DD098D20-1850-4fc8-8E35-EA24A1BEFF5E}. |

## <a name="BKMK_examples"></a>Esempi
Per aggiungere un pacchetto di driver a un'immagine di avvio, digitare uno dei seguenti:
```
wdsutil /add-ImageDriverPackagmedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /DriverPackage:XYZ
```
```
wdsutil /verbose /add-ImageDriverPackagmedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /PackageId:{4D36E972-E325-11CE-Bfc1-08002BE10318}
```
#### <a name="additional-references"></a>riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando add-ImageDriverPackages](using-the-add-imagedriverpackages-command.md)
