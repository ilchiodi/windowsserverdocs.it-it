---
title: Remove-DriverGroup
description: Argomento dei comandi di Windows per Remove-DriverGroup, che rimuove un gruppo di driver da un server.
ms.prod: windows-server
ms.topic: article
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 56622c30b8b0af88a57c476eb4f03d598703d603
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830524"
---
# <a name="remove-drivergroup"></a>Remove-DriverGroup

Rimuove un gruppo di driver da un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/DriverGroup: nome gruppo\<>|Specifica il nome del gruppo di driver da rimuovere.|
|[/Server:\<nome server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per rimuovere un gruppo di driver, digitare uno dei seguenti:
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)