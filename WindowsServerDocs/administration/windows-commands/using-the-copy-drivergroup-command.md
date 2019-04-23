---
title: Utilizzando il comando copia DriverGroup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68d9c6f4ca78991bb4c286042a6172211161dd1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842082"
---
# <a name="using-the-copy-drivergroup-command"></a>Utilizzando il comando copia DriverGroup



Duplica un gruppo di driver esistente nel server tra cui i filtri pacchetti driver e lo stato abilitato o disabilitato.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|[/ Server:\<nome Server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/ DriverGroup:\<nome del gruppo di origine >|Specifica il nome del gruppo di driver di origine.|
|/ GroupName:\<nuovo nome del gruppo >|Specifica il nome del nuovo gruppo di driver.|

## <a name="BKMK_examples"></a>Esempi

Per copiare un gruppo di driver, digitare uno dei seguenti:
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)