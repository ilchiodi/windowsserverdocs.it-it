---
title: Utilizzando il comando add-DriverGroupFilter
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a66c5e68-99ea-4e47-b68d-8109633ae336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45525c01a6f2cdfbfd17000368356dda72ed29bc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440738"
---
# <a name="using-the-add-drivergroupfilter-command"></a>Utilizzando il comando add-DriverGroupFilter



Aggiunge un filtro a un gruppo di driver in un server.

## <a name="syntax"></a>Sintassi

```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:<Group Name> [/Server:<Server name>] /FilterType:<Filter Type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]
```

## <a name="parameters"></a>Parametri

|         Parametro          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| / DriverGroup:\<nome gruppo > |                                                                                                                                                                                                                                                                                                                                                                                                                                                              Specifica il nome del gruppo di driver.                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|  [/ Server:\<nome Server >]  |                                                                                                                                                                                                                                                                                                                                                                                                               Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.                                                                                                                                                                                                                                                                                                                                                                                                               |
| / FilterType:\<TipoFiltro >  |                                                                                                                                                                                                   Specifica il tipo di filtro da aggiungere al gruppo. È possibile specificare più tipi di filtro in un unico comando. Ogni tipo di filtro deve essere seguita da **/Policy** e includere almeno una **/valore**. \<Valore di FilterType > può essere **BiosVendor**, **BiosVersion**, **ChassisType**, **produttore**, **Uuid**, **OsVersion**, **OsEdition**, o **OsLanguage**. Per informazioni su come ottenere i valori per tutti gli altri tipi di filtro, vedere [filtri di gruppo di Driver](https://go.microsoft.com/fwlink/?LinkID=155158) (<https://go.microsoft.com/fwlink/?LinkID=155158>).                                                                                                                                                                                                    |
|     [/ Criteri: {includono      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             Escludere}]                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|     [/ Value:\<valore >]      | Specifica il valore di client che corrisponde a **/FilterType**. È possibile specificare più valori per un singolo tipo. Vedere di seguito sono elencati i valori validi per **ChassisType**. Per informazioni su come ottenere i valori per tutti gli altri tipi di filtro, vedere [filtri di gruppo di Driver](https://go.microsoft.com/fwlink/?LinkID=155158) (<https://go.microsoft.com/fwlink/?LinkID=155158>).</br>**Altri**</br>**UnknownChassis**</br>**Desktop**</br>**LowProfileDesktop**</br>**PizzaBox**</br>**MiniTower**</br>**Tower**</br>**Portabile**</br>**Computer portatile**</br>**taccuino**</br>**Handheld**</br>**DockingStation**</br>**AllInOne**</br>**SubNotebook**</br>**SpaceSaving**</br>**LunchBox**</br>**MainSystemChassis**</br>**ExpansionChassis**</br>**SubChassis**</br>**BusExpansionChassis**</br>**PeripheralChassis**</br>**StorageChassis**</br>**RackMountChassis**</br>**SealedCaseComputer**</br>**MultiSystemChassis**</br>**CompactPci**</br>**AdvancedTca** |

## <a name="BKMK_examples"></a>Esempi

Per aggiungere un filtro a un gruppo di driver, digitare uno dei seguenti:
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /Value:Name2
```
```
WDSUTIL /Add-DriverGroupFilter /DriverGroup:PrinterDrivers /FilterType:Manufacturer /Policy:Include /Value:Name1 /FilterType:ChassisType /Policy:Exclude /Value:Tower /Value:MiniTower
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

