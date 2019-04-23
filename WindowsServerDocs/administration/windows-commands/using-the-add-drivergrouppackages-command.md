---
title: Utilizzando il comando add-DriverGroupPackages
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 29022f53-ce14-4b2d-a81a-679c18e022b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad0f8c7ed202f0d6fccc11307b17b742c70c3e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842792"
---
# <a name="using-the-add-drivergrouppackages-command"></a>Utilizzando il comando add-DriverGroupPackages

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_examples).
## <a name="syntax"></a>Sintassi
```
wdsutil /add-DriverGroupPackages /DriverGroup:<Group Name> [/Server:<Server Name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value>
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/DriverGroup:<Group Name>|Specifica il nome del gruppo di driver.|
|/Server:<Server name>|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/ Tipo di filtro:<Filter type>|Specifica l'attributo del pacchetto driver per la ricerca. È possibile specificare più attributi in un unico comando. È inoltre necessario specificare /Operator e /Value con questa opzione.<br /><br /><Filter type> Può essere uno dei seguenti:<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/ Operatore: {uguale & #124; NotEqual & #124; GreaterOrEqual & #124; LessOrEqual & #124; Contiene}|Specifica la relazione tra l'attributo e i valori. È possibile specificare solo **Contains** con gli attributi di stringa. È possibile specificare solo **uguale**, **NotEqual**, **GreaterOrEqual** e **LessOrEqual** con gli attributi di data e versione.|
|-Valore:<Value>|Specifica il valore di client corrispondente **/Filtertype**. È possibile specificare più valori per una singola **/Filtertype**. Nell'elenco seguente vengono illustrati i valori che è possibile specificare per ogni filtro. Per altre informazioni su questi valori, vedere [attributi di Driver e pacchetti](https://go.microsoft.com/fwlink/?LinkId=166895) (https://go.microsoft.com/fwlink/?LinkId=166895).<br /><br />-PackageId - specificare un GUID valido. Ad esempio: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageName specificare qualsiasi valore stringa.<br />-Specificare PackageEnabled - **Sì** o **n**.<br />-Packagedateadded - specificare la data nel formato seguente: AAAA/MM/GG<br />-PackageInfFilename specificare qualsiasi valore stringa.<br />-PackageClass - specificare un nome di classe valido o GUID della classe. Ad esempio: **DiskDrive**, **Net**, o {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageProvider specificare qualsiasi valore stringa.<br />-Specificare PackageArchitecture - **x86**, **x64**, o **ia64**.<br />-PackageLoale - specificare un identificatore di lingua valido. Ad esempio: **en-US** o **es-ES**.<br />-Specificare PackageSigned - **Sì** o **n**.<br />-PackagedatePublished - specificare la data nel formato seguente: AAAA/MM/GG<br />-Packageversion - specificare la versione nel formato seguente: a.b.x.y. Ad esempio: 6.1.0.0<br />-Driverdescription specificare qualsiasi valore stringa.<br />-DriverManufacturer specificare qualsiasi valore stringa.<br />-DriverHardwareId - specificare qualsiasi valore stringa.<br />-DrivercompatibleId - specificare qualsiasi valore stringa.<br />-DriverExcludeId - specificare qualsiasi valore stringa.<br />-DriverGroupId - specificare un GUID valido. Ad esempio: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-DriverGroupName specificare qualsiasi valore stringa.|
## <a name="BKMK_examples"></a>Esempi
Per aggiungere un pacchetto driver, digitare uno dei seguenti:
```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:printerdrivers /Filtertype:PackageClass /Operator:Equal /Value:printer /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2
```
```
wdsutil /verbose /add-DriverGroupPackages /DriverGroup:DisplayDriversX86 /Filtertype:PackageClass /Operator:Equal /Value:Display /Filtertype:PackageArchitecture /Operator:Equal /Value:x86 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando add-DriverGroupPackage](using-the-add-drivergrouppackage-command.md)
[utilizzando il comando Aggiungi /InfFile](using-the-add-driverpackage-command.md)
[utilizzando il sottocomando AllDriverPackages Aggiungi](using-the-add-alldriverpackages-subcommand.md)