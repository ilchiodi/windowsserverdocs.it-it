---
title: Utilizzando il comando remove-DriverGroupFilter
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 837bd5d4-c79d-4714-942d-9875bd8e61dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a546ead7220273955368c582ac1e3f9b3f61c191
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883432"
---
# <a name="using-the-remove-drivergroupfilter-command"></a>Utilizzando il comando remove-DriverGroupFilter



Rimuove una regola di filtro da un gruppo di driver in un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/ DriverGroup:\<nome gruppo >|Specifica il nome del gruppo di driver.|
|[/ Server:\<nome Server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|
|[/ FilterType:\<TipoFiltro >]|Specifica il tipo di filtro da rimuovere dal gruppo. \<Valore di FilterType > può essere uno dei seguenti:</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Produttore**</br>**Uuid**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|

## <a name="BKMK_examples"></a>Esempi

Per rimuovere un filtro, digitare uno dei seguenti:
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer
```
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /FilterType:OSLanguage
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)