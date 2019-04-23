---
title: Utilizzando il comando add-DriverGroup
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a92fe8f-03f9-462a-b99e-f23275259807
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 322a9a671f90bf56f6357289f7727c142a0145cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825092"
---
# <a name="using-the-add-drivergroup-command"></a>Utilizzando il comando add-DriverGroup

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge un gruppo di driver al server.
Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_examples).
## <a name="syntax"></a>Sintassi
```
wdsutil /add-DriverGroup /DriverGroup:<Group Name>\n\
[/Server:<Server name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}] [/Filtertype:<Filter type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/DriverGroup:<Group Name>|Specifica il nome del nuovo gruppo di driver.|
|/Server:<Server name>|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se viene specificato alcun nome di server, viene utilizzato il server locale.|
|/ Abilitato: {Sì & #124; N}|Abilita o disabilita il pacchetto.|
|/ Applicabilità: {corrispondente & #124; All}|Specifica che i pacchetti per l'installazione se vengono soddisfatti i criteri di filtro. **Matched** significa installa solo i pacchetti di driver che corrispondono a un hardware client s. **Tutti** significa installa tutti i pacchetti ai client indipendentemente dall'hardware.|
|/ Tipo di filtro:<Filtertype>|Specifica il tipo di filtro da aggiungere al gruppo. È possibile specificare più tipi di filtro in un unico comando. Ogni tipo di filtro deve essere seguita da **/Policy** e almeno un **/valore**. <Filtertype> Può essere uno dei seguenti:<br /><br />**BiosVendor**<br /><br />**BIOSVersion**<br /><br />**ChassisType**<br /><br />**Produttore**<br /><br />**Uuid**<br /><br />**Versione del sistema operativo**<br /><br />**Osedition**<br /><br />**OsLanguage**<br /><br />Per informazioni su come ottenere i valori per tutti gli altri tipi di filtro, vedere [filtri di gruppo di Driver](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).|
|[/ Criteri: {includono & #124; Escludere}]|Specifica i criteri da impostare per il filtro. Se **/Policy** è impostato su **Include**, i computer client che corrispondono al filtro sono autorizzati a installare i driver di questo gruppo. Se **/Policy** è impostato su **escludere**, quindi i computer client che corrispondono al filtro non sono autorizzati a installare i driver di questo gruppo.|
|[/ Valore:<Value>]|Specifica il valore di client che corrisponde a **/Filtertype**. È possibile specificare più valori per un singolo tipo. Vedere di seguito sono elencati i valori validi per determinati tipi di filtro. Gli attributi seguenti sono per **Chassistype**. Per informazioni su come ottenere i valori per tutti gli altri tipi di filtro, vedere [filtri di gruppo di Driver](https://go.microsoft.com/fwlink/?LinkID=155158) (https://go.microsoft.com/fwlink/?LinkID=155158).<br /><br />**Altri**<br /><br />**UnknownChassis**<br /><br />**Desktop**<br /><br />**LowProfileDesktop**<br /><br />**PizzaBox**<br /><br />**MiniTower**<br /><br />**Tower**<br /><br />**Portabile**<br /><br />**Computer portatile**<br /><br />**taccuino**<br /><br />**Handheld**<br /><br />**DockingStation**<br /><br />**AllInOne**<br /><br />**SubNotebook**<br /><br />**SpaceSaving**<br /><br />**LunchBox**<br /><br />**MainSystemChassis**<br /><br />**ExpansionChassis**<br /><br />**SubChassis**<br /><br />**BusExpansionChassis**<br /><br />**PeripheralChassis**<br /><br />**StoraeChassis**<br /><br />**RackmountChassis**<br /><br />**SealedCasecomputer**<br /><br />**MultiSystemChassis**<br /><br />**compactPci**<br /><br />**AdvancedTca**|
## <a name="BKMK_examples"></a>Esempi
Per aggiungere un gruppo di driver, digitare uno dei seguenti:
```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Applicability:All /Filtertype:Manufacturer /Policy:Include /Value:Name1 /Filtertype:Chassistype /Policy:Exclude /Value:Tower /Value:MiniTower
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando add-DriverGroupPackage](using-the-add-drivergrouppackage-command.md)
[utilizzando il comando add-DriverGroupPackages](using-the-add-drivergrouppackages-command.md)
[utilizzando il comando add-DriverGroupFilter](using-the-add-drivergroupfilter-command.md)