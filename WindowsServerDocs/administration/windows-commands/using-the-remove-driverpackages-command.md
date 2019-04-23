---
title: Utilizzando il comando remove-DriverPackages
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a527084b-305e-4d3d-95c3-4f5a5ea0637b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 166602bcb1203f0ecb870ed04888ca0051f16827
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872482"
---
# <a name="using-the-remove-driverpackages-command"></a>Utilizzando il comando remove-DriverPackages

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove i pacchetti driver dal server.
## <a name="syntax"></a>Sintassi
```
wdsutil /remove-DriverPackages [/Server:<Server name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|
|/ Tipo di filtro:<Filter type>|Specifica l'attributo del pacchetto driver per la ricerca. È possibile specificare più attributi in un unico comando. È inoltre necessario specificare **/Operator** e **/valore** con questa opzione.<br /><br /><Filter type> Può essere uno dei seguenti:<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/ Operatore: {uguale & #124; NotEqual & #124; GreaterOrEqual & #124; LessOrEqual & #124; Contiene}|Specifica la relazione tra l'attributo e i valori. È possibile specificare solo **Contains** con gli attributi di stringa. È possibile specificare solo **GreaterOrEqual** e **LessOrEqual** con gli attributi di data e versione.|
|-Valore:<Value>|Specifica il valore da cercare specificato <attribute>. È possibile specificare più valori per una singola **/Filtertype**. Nell'elenco seguente vengono descritti gli attributi che è possibile specificare per ogni filtro. Per altre informazioni su questi attributi, vedere [attributi di Driver e pacchetti](https://go.microsoft.com/fwlink/?LinkId=166895) (https://go.microsoft.com/fwlink/?LinkId=166895).<br /><br />-PackageId - specificare un GUID valido. Ad esempio: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageName specificare qualsiasi valore stringa.<br />-Specificare PackageEnabled - **Sì** o **n**.<br />-Packagedateadded - specificare la data nel formato seguente: AAAA/MM/GG<br />-PackageInfFilename specificare qualsiasi valore stringa.<br />-PackageClass - specificare un nome di classe valido o GUID della classe. Ad esempio:  **DiskDrive**, **Net**, o {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageProvider specificare qualsiasi valore stringa.<br />-Specificare PackageArchitecture - **x86**, **x64**, o **ia64**.<br />-PckageLocale - specificare un identificatore di lingua valido. Ad esempio: **en-US** o **es-ES**.<br />-Specificare PackageSigned - **Sì** o **n**.<br />-PackagedatePublished - specificare la data nel formato seguente: AAAA/MM/GG<br />-Packageversion - specificare la versione nel formato seguente: a.b.x.y. Ad esempio:  6.1.0.0<br />-Driverdescription specificare qualsiasi valore stringa.<br />-DriverManufacturer specificare qualsiasi valore stringa.<br />-DriverHardwareId - specificare qualsiasi valore stringa.<br />-DrivercompatibleId - specificare qualsiasi valore stringa.<br />-DriverExcludeId - specificare qualsiasi valore stringa.<br />-DriverGroupId - specificare un GUID valido. Ad esempio: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-DriverGroupName specificare qualsiasi valore stringa.|
## <a name="BKMK_examples"></a>Esempi
Per rimuovere i pacchetti, digitare uno dei seguenti:
```
wdsutil /verbose /remove-DriverPackages /Server:MyWdsServer
/Filtertype:PackageProvider /Operator:Equal /Value:Name1 /Value:Name2
```
```
wdsutil /remove-DriverPackages /Filtertype:PackageArchitecture /Operator:Equal
/Value:x86 /Value:x64 /Filtertype:PackageEnabled /Operator:Equal /Value:No
```
```
wdsutil /verbose /remove-DriverPackages /Server:MyWdsServer
/Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando remove /InfFile](using-the-remove-driverpackage-command.md)
