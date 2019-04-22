---
title: Utilizzando il comando add-ImageDriverPackages
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9dc78909-a4d1-42a2-af8f-21ebcbfe8302
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55b26acf9c4006db3d64e27be8a10e158876ddc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817362"
---
# <a name="using-the-add-imagedriverpackages-command"></a>Utilizzando il comando add-ImageDriverPackages

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di aggiungere pacchetti driver dall'archivio driver a un'immagine di avvio. La versione di immagine deve essere Windows 7 o Windows Server 2008 R2 o versioni successive.
Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_examples).
## <a name="syntax"></a>Sintassi
```
wdsutil /add-ImageDriverPackages [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
[/Filename:<File name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/Server:<Server name>|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.|
media:<Image name>|Specifica il nome dell'immagine da aggiungere il driver.|
mediatype:Boot|Specifica il tipo di immagine per aggiungere il driver. Pacchetti driver possono essere aggiunto solo a immagini d'avvio.|
|/ Architettura: {x86 & #124; ia64 & #124; x64}|Specifica l'architettura dell'immagine di avvio. Poiché è possibile che lo stesso nome di immagine per immagini di avvio diverse architetture, è necessario specificare l'architettura per garantire che venga utilizzata l'immagine corretta.|
|/Filename:<File name>|Specifica il nome del file. Se l'immagine non può essere identificata in modo univoco dal nome, è necessario specificare il nome del file.|
|/ Tipo di filtro:<Filter type>|Specifica l'attributo del pacchetto driver per la ricerca. È possibile specificare più attributi in un unico comando. È inoltre necessario specificare **/Operator** e **/valore** con questa opzione.<br /><br /><Filter type> Può essere uno dei seguenti:<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/ Operatore: {uguale & #124; NotEqual & #124; GreaterOrEqual & #124; LessOrEqual & #124; Contiene}|La relazione tra l'attributo e i valori. È possibile specificare solo **Contains** con gli attributi di stringa. È possibile specificare solo **GreaterOrEqual** e **LessOrEqual** con gli attributi di data e versione.|
|-Valore:<Value>|Specifica il valore da cercare relativo specificato <attribute>. È possibile specificare più valori per una singola **/Filtertype**. Nell'elenco seguente contorni gli attributi che è possibile specificare per ogni filtro. Per altre informazioni su questi attributi, vedere [attributi di Driver e pacchetti](https://go.microsoft.com/fwlink/?LinkId=166895) (https://go.microsoft.com/fwlink/?LinkId=166895).<br /><br />-PackageId - specificare un GUID valido. Ad esempio: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageName specificare qualsiasi valore stringa.<br />-Specificare PackageEnabled - **Sì** o **n**.<br />-Packagedateadded - specificare la data nel formato seguente: AAAA/MM/GG<br />-PackageInfFilename specificare qualsiasi valore stringa.<br />-PackageClass - specificare un nome di classe valido o GUID della classe. Ad esempio:  **DiskDrive**, **Net**, o {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageProvider specificare qualsiasi valore stringa.<br />-Specificare PackageArchitecture - **x86**,  **x64**, o **ia64**.<b />-PackageLocale - specificare un identificatore di lingua valido. Ad esempio: **en-US** o **es-ES**.<br />-Specificare PackageSigned - **Sì** o **n**.<br />-PackagedatePublished - specificare la data nel formato seguente: AAAA/MM/GG<br />-Packageversion - specificare la versione nel formato seguente: a.b.x.y. Ad esempio:  6.1.0.0<br />-Driverdescription specificare qualsiasi valore stringa.<br />-DriverManufacturer specificare qualsiasi valore stringa.<br />-DriverHardwareId - specificare qualsiasi valore stringa.<br />-DrivercompatibleId - specificare qualsiasi valore stringa.<br />-DriverExcludeId - specificare qualsiasi valore stringa.<br />-DriverGroupId - specificare un GUID valido. Ad esempio: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-DriverGroupName specificare qualsiasi valore stringa.|
## <a name="BKMK_examples"></a>Esempi
Per aggiungere pacchetti driver a un'immagine di avvio, digitare uno dei seguenti:
```
wdsutil /add-ImageDriverPackagemedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /Filtertype:DriverGroupName /Operator:Equal /Value:x86Bus /Filtertype:PackageProvider /Operator:Contains /Value:Provider1 /Filtertype:Packageversion /Operator:GreaterOrEqual /Value:6.1.0.0
```
```
wdsutil /verbose /add-ImageDriverPackagemedia: "WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
```
wdsutil /verbose /add-ImageDriverPackagemedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Value:System /Value:DiskDrive /Value:HDC /Value:SCSIAdapter
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando add-ImageDriverPackage](using-the-add-imagedriverpackage-command.md)
[utilizzando il sottocomando Aggiungi AllDriverPackages](using-the-add-alldriverpackages-subcommand.md)
