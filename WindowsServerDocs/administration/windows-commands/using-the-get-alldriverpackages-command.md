---
title: Utilizzando il comando get-AllDriverPackages
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9eb8fcb7-ef46-4c8d-9623-8969a3c8b8a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e531d9e175ba35aa092c1ef95ec5fe7d1eddff6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843652"
---
# <a name="using-the-get-alldriverpackages-command"></a>Utilizzando il comando get-AllDriverPackages

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza informazioni su tutti i pacchetti di driver in un server che soddisfano i criteri di ricerca specificati.
## <a name="syntax"></a>Sintassi
```
wdsutil /Get-AllDriverPackages [/Server:<Server name>] [/Show:{Drivers | Files | All}] [/Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|
|[/Show: {driver & #124; File & #124; All}]|Indica le informazioni sul pacchetto da visualizzare. Se **/Mostra** non viene specificato, il valore predefinito è per restituire solo i driver dei metadati del pacchetto. **I driver** Visualizza l'elenco dei driver nel pacchetto. **File** consente di visualizzare l'elenco dei file nel pacchetto. **Tutti** Visualizza driver e file.|
|/ Tipo di filtro:<Filter type>|Specifica l'attributo del pacchetto driver per la ricerca. È possibile specificare più attributi in un unico comando. È inoltre necessario specificare **/Operator** e **/valore** con questa opzione.<br /><br /><Filter type> Può essere uno dei seguenti:<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/ Operatore: {uguale & #124; NotEqual & #124; GreaterOrEqual & #124; LessOrEqual & #124; Contiene}|Specifica la relazione tra l'attributo e i valori. È possibile specificare **Contains** solo con gli attributi di stringa. È possibile specificare **GreaterOrEqual** e **LessOrEqual** solo con gli attributi di data e versione.|
|-Valore:<Value>|Specifica il valore per la ricerca su specificato <attribute>.  È possibile specificare più valori per una singola **/Filtertype**. Nell'elenco seguente vengono descritti gli attributi che è possibile specificare per ogni filtro. Per altre informazioni su questi attributi, vedere [attributi di Driver e pacchetti](https://go.microsoft.com/fwlink/?LinkId=166895) (https://go.microsoft.com/fwlink/?LinkId=166895).<br /><br />-PackageId - specificare un GUID valido. Ad esempio: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageName specificare qualsiasi valore stringa.<br />-Specificare PackageEnabled - **Sì** o **n**.<br />-Packagedateadded - specificare la data nel formato seguente: AAAA/MM/GG<br />-PackageInfFilename specificare qualsiasi valore stringa.<br />-PackageClass - specificare un nome di classe valido o GUID della classe. Ad esempio:  **DiskDrive**, **Net**, o {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageProvider specificare qualsiasi valore stringa.<br />-Specificare PackageArchitecture - **x86**, **x64**, o **ia64**.<br />-PackagLocale - specificare un identificatore di lingua valido. Ad esempio: **en-US** o **es-ES**.<br />-Specificare PackageSigned - **Sì** o **n**.<br />-PackagedatePublished - specificare la data nel formato seguente: AAAA/MM/GG<br />-Packageversion specificare la versione nel formato seguente: a.b.x.y. Ad esempio:  6.1.0.0<br />-Driverdescription specificare qualsiasi valore stringa.<br />-DriverManufacturer specificare qualsiasi valore stringa.<br />-DriverHardwareId - specificare qualsiasi valore stringa.<br />-DrivercompatibleId - specificare qualsiasi valore stringa.<br />-DriverExcludeId specificare qualsiasi valore stringa.<br />-DriverGroupId specificare un GUID valido. Ad esempio: {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-DriverGroupName specificare qualsiasi valore stringa.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare informazioni, digitare uno dei seguenti:
```
wdsutil /Get-AllDriverPackages /Server:MyWdsServer /Show:All /Filtertype:DriverGroupName /Operator:Contains /Value:printer /Filtertype:PackageArchitecture /Operator:Equal /Value:x64 /Value:x86
```
```
wdsutil /Get-AllDriverPackages /Show:Drivers /Filtertype:Packagedateadded /Operator:GreaterOrEqual /Value:2008/01/01
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando get /InfFile](using-the-get-driverpackage-command.md)
[utilizzando il comando get-DriverPackageFile](using-the-get-driverpackagefile-command.md)
