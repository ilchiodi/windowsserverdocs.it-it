---
title: Sottocomando set-DriverGroupFilter
description: Argomento di riferimento per sottocomando set-DriverGroupFilter, che aggiunge o rimuove un filtro di gruppo di driver esistente da un gruppo di driver.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8a8d3567785b54a722b1787c519de7eb0b7a229
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721728"
---
# <a name="subcommand-set-drivergroupfilter"></a>Sottocomando: set-DriverGroupFilter

Aggiunge o rimuove un filtro di gruppo di driver esistente da un gruppo di driver.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

### <a name="parameters"></a>Parametri

|         Parametro          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /DriverGroup:\<nome del gruppo> |                                                                                                                                                                                                                                                                                                                                                                                                                                                                 Specifica il nome del gruppo di driver.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|  [/Server:\<nome server>]  |                                                                                                                                                                                                                                                                                                                                                                                                                Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| /FilterType:\<FilterType>  |                                                                                                                                                                                                                                                                       Specifica il tipo di filtro di gruppo di driver da aggiungere o rimuovere. È possibile specificare più filtri in un unico comando. Per ogni **/FilterType**, è possibile aggiungere o rimuovere più valori utilizzando **/RemoveValue** e **/AddValue**. \<Il> FilterType può essere uno dei seguenti:</br>**Biostatore**</br>**BiosVersion**</br>**ChassisType**</br>**Produttore**</br>**UUID**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**                                                                                                                                                                                                                                                                        |
|     [/Policy: {include      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                Escludi}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|    [/AddValue:\<valore>]    | Specifica il nuovo valore di client da aggiungere al filtro. È possibile specificare più valori per un tipo di filtro. Vedere di seguito sono elencati i valori di attributo validi per **ChassisType**. Per informazioni su come ottenere i valori per tutti gli altri tipi di filtro, vedere filtri<https://go.microsoft.com/fwlink/?LinkID=155158>dei gruppi di [driver](https://go.microsoft.com/fwlink/?LinkID=155158) ().</br>**Altro**</br>**UnknownChassis**</br>**Desktop**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Tower**</br>**Portabile**</br>**Laptop**</br>**Notebook**</br>**Palmari**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |
|  [/RemoveValue:\<valore>]   |                                                                                                                                                                                                                                                                                                                                                                                                                                     Specifica il valore di client esistenti per rimuovere il filtro specificato con **/AddValue**.                                                                                                                                                                                                                                                                                                                                                                                                                                      |

## <a name="examples"></a>Esempi

Per rimuovere un filtro, digitare uno dei seguenti:
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)