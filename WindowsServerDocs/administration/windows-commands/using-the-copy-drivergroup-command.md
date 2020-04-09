---
title: Copy-DriverGroup
description: Windows Commands argomento for Copy-DriverGroup, che duplica un gruppo di driver esistente nel server, inclusi i filtri, i pacchetti driver e lo stato abilitato/disabilitato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 277903150a25555b03b51c980436250656c597b1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831734"
---
# <a name="copy-drivergroup"></a>Copy-DriverGroup

Duplica un gruppo di driver esistente nel server tra cui i filtri pacchetti driver e lo stato abilitato o disabilitato.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[/Server:\<nome server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/DriverGroup:\<nome del gruppo di origine >|Specifica il nome del gruppo di driver di origine.|
|/GroupName:\<nuovo nome del gruppo >|Specifica il nome del nuovo gruppo di driver.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per copiare un gruppo di driver, digitare uno dei seguenti:
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)