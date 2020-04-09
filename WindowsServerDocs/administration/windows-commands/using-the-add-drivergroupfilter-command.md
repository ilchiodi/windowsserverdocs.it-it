---
title: Add-DriverGroupFilter
description: Windows Commands argomento per Add-DriverGroupFilter, che aggiunge un filtro a un gruppo di driver in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a62baf85462d4340d61196bc154efd6d852f1f7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832114"
---
# <a name="add-drivergroupfilter"></a>Add-DriverGroupFilter

Aggiunge un filtro a un gruppo di driver in un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

### <a name="parameters"></a>Parametri

|         Parametro          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup: nome gruppo\<> |                                                                                                                                                                                                                                                                                                                                                                                                                                                              Specifica il nome del gruppo di driver.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|  [/Server:\<nome server >]  |                                                                                                                                                                                                                                                                                                                                                                                                               Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.                                                                                                                                                                                                                                                                                                                                                                                                               |
| /FilterType:\<FilterType >  |                                                                                                                                                                                                   Specifica il tipo di filtro da aggiungere al gruppo. È possibile specificare più tipi di filtro in un unico comando. Ogni tipo di filtro deve essere seguita da **/Policy** e includere almeno una **/valore**. \<FilterType > **può essere** **bioBiosVersion**, **ChassisType**, **produttore**, **UUID**, **OsVersion**, **OsEdition**o **OsLanguage**. Per informazioni su come ottenere i valori per tutti gli altri tipi di filtro, vedere [filtri dei gruppi di driver](https://go.microsoft.com/fwlink/?LinkID=155158) (<https://go.microsoft.com/fwlink/?LinkID=155158>).                                                                                                                                                                                                    |
|     [/Policy: {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Escludi}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/Value: valore\<>]      | Specifica il valore di client che corrisponde a **/FilterType**. È possibile specificare più valori per un singolo tipo. Vedere di seguito sono elencati i valori validi per **ChassisType**. Per informazioni su come ottenere i valori per tutti gli altri tipi di filtro, vedere [filtri dei gruppi di driver](https://go.microsoft.com/fwlink/?LinkID=155158) (<https://go.microsoft.com/fwlink/?LinkID=155158>).</br>**Altri**</br>**UnknownChassis**</br>**Desktop**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Tower**</br>**Portabile**</br>**Portatile**</br>**Notebook**</br>**Palmare**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**Sottochassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per aggiungere un filtro a un gruppo di driver, digitare uno dei seguenti:
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

