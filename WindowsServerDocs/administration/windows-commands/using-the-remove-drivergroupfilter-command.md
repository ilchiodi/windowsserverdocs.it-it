---
title: Remove-DriverGroupFilter
description: Argomento dei comandi di Windows per Remove-DriverGroupFilter, che rimuove una regola di filtro da un gruppo di driver in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 837bd5d4-c79d-4714-942d-9875bd8e61dc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08914b66c37d327ddef2a50d2f98adcfdbb88ffe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830494"
---
# <a name="remove-drivergroupfilter"></a>Remove-DriverGroupFilter



Rimuove una regola di filtro da un gruppo di driver in un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/DriverGroup: nome gruppo\<>|Specifica il nome del gruppo di driver.|
|[/Server:\<nome server >]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|
|[/FilterType:\<FilterType >]|Specifica il tipo di filtro da rimuovere dal gruppo. \<FilterType > può essere uno dei seguenti:</br>**Biostatore**</br>**BiosVersion**</br>**ChassisType**</br>**Produttore**</br>**UUID**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per rimuovere un filtro, digitare uno dei seguenti:
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer
```
```
WDSUTIL /Remove-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /FilterType:OSLanguage
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)