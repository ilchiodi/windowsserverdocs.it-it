---
title: Remove-DriverGroupPackage
description: Argomento comandi di Windows per Remove-DriverGroupPackage, che rimuove un pacchetto driver da un gruppo di driver in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2e48616d-d6a4-45f0-a5c6-efe62bf6a0ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9dd30f430bd91bf2680cd1138270526bce965f9d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830464"
---
# <a name="remove-drivergrouppackage"></a>Remove-DriverGroupPackage



Rimuove un pacchetto driver da un gruppo di driver in un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:<Group Name> [/Server:<Server Name>] {/DriverPackage:<Name> | /PackageId:<ID>}
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[/Server:\<nome server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|
|[/DriverPackage: nome\<>]|Specifica il nome del pacchetto driver da rimuovere.|
|[/PackageId: ID\<>]|Specifica l'ID di servizi di distribuzione Windows del pacchetto driver da rimuovere. È necessario specificare questa opzione se il pacchetto driver non può essere identificato in modo univoco in base al nome.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318}
```
```
WDSUTIL /Remove-DriverGroupPackage /DriverGroup:PrinterDrivers /DriverPackage:XYZ
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)