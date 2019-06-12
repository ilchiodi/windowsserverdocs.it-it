---
title: Il sottocomando set-DriverGroupFilter
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 829ab1f0-4514-421e-9cc0-767b238da69c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5423b6e7444c01c5889a5b207a545d1761a73402
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441143"
---
# <a name="subcommand-set-drivergroupfilter"></a>Sottocomando: set-DriverGroupFilter



Aggiunge o rimuove un filtro di gruppo di driver esistente da un gruppo di driver.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> [/Policy:{Include | Exclude}] [/AddValue:<Value> [/AddValue:<Value> ...]] [/RemoveValue:<Value> [/RemoveValue:<Value> ...]]
```

## <a name="parameters"></a>Parametri

|         Parametro          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| / DriverGroup:\<nome gruppo > |                                                                                                                                                                                                                                                                                                                                                                                                                                                                 Specifica il nome del gruppo di driver.                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|  [/ Server:\<nome Server >]  |                                                                                                                                                                                                                                                                                                                                                                                                                Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| / FilterType:\<TipoFiltro >  |                                                                                                                                                                                                                                                                       Specifica il tipo di filtro di gruppo di driver da aggiungere o rimuovere. È possibile specificare più filtri in un unico comando. Per ogni **/FilterType**, è possibile aggiungere o rimuovere più valori utilizzando **/RemoveValue** e **/AddValue**. \<Valore di FilterType > può essere uno dei seguenti:</br>**BiosVendor**</br>**BiosVersion**</br>**ChassisType**</br>**Produttore**</br>**Uuid**</br>**OsVersion**</br>**OsEdition**</br>**OsLanguage**                                                                                                                                                                                                                                                                        |
|     [/ Criteri: {includono      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                Escludere}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|    [/AddValue:\<Value>]    | Specifica il nuovo valore di client da aggiungere al filtro. È possibile specificare più valori per un tipo di filtro. Vedere di seguito sono elencati i valori di attributo validi per **ChassisType**. Per informazioni su come ottenere i valori per tutti gli altri tipi di filtro, vedere [filtri di gruppo di Driver](https://go.microsoft.com/fwlink/?LinkID=155158) (<https://go.microsoft.com/fwlink/?LinkID=155158>).</br>**Altri**</br>**UnknownChassis**</br>**Desktop**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Tower**</br>**Portabile**</br>**Computer portatile**</br>**taccuino**</br>**Handheld**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |
|  [/RemoveValue:\<Value>]   |                                                                                                                                                                                                                                                                                                                                                                                                                                     Specifica il valore di client esistenti per rimuovere il filtro specificato con **/AddValue**.                                                                                                                                                                                                                                                                                                                                                                                                                                      |

## <a name="BKMK_examples"></a>Esempi

Per rimuovere un filtro, digitare uno dei seguenti:
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /AddValue:Name1 /RemoveValue:Name2
```
```
WDSUTIL /Set-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /RemoveValue:Name1 /FilterType:ChassisType /Policy:Exclude /AddValue:Tower /AddValue:MiniTower
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)