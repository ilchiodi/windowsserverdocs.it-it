---
title: Utilizzando il comando remove-DriverGroup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b56f162861caf4493550f9e063065e9544e52eae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885752"
---
# <a name="using-the-remove-drivergroup-command"></a>Utilizzando il comando remove-DriverGroup



Rimuove un gruppo di driver da un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/ DriverGroup:\<nome gruppo >|Specifica il nome del gruppo di driver da rimuovere.|
|[/ Server:\<nome Server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|

## <a name="BKMK_examples"></a>Esempi

Per rimuovere un gruppo di driver, digitare uno dei seguenti:
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)