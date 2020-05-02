---
title: Copy-DriverGroup
description: Argomento di riferimento per Copy-DriverGroup, che duplica un gruppo di driver esistente nel server, inclusi i filtri, i pacchetti driver e lo stato abilitato/disabilitato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc157e9ef6d07a45efe2a19221fb3a046b2f65c1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721007"
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
|[/Server:\<nome server>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/DriverGroup:\<nome del gruppo di origine>|Specifica il nome del gruppo di driver di origine.|
|/GroupName:\<nuovo nome gruppo>|Specifica il nome del nuovo gruppo di driver.|

## <a name="examples"></a>Esempi

Per copiare un gruppo di driver, digitare uno dei seguenti:
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)