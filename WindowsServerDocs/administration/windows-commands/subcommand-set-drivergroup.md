---
title: Sottocomando set-DriverGroup
description: Windows Commands argomento per sottocommand set-DriverGroup, che imposta le proprietà di un gruppo di driver esistente in un server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e4ba9b1c-8c52-4fd5-969b-f7905611b364
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c49381dc65f3b2ffc9a04e4fb2699818515a931f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834034"
---
# <a name="subcommand-set-drivergroup"></a>Sottocomando: set-DriverGroup

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta le proprietà di un gruppo di driver esistente in un server.

## <a name="syntax"></a>Sintassi
```
wdsutil /Set-DriverGroup /DriverGroup:<Group Name> [/Server:<Server Name>] [/Name:<New Group Name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/DriverGroup:<Group Name>|Specifica il nome del gruppo di driver.|
|[/Server:<Server name>]|Specifica il nome del server. Questo può essere il nome NetBIOS o il nome FQDN. Se non viene specificato un nome di server, viene utilizzato il server locale.|
|[/Name:<New Group Name>]|Specifica il nuovo nome per il gruppo di driver.|
|[O abilitati: {Sì & #124; N}|Abilita o disabilita il gruppo di driver.|
|[/ Applicabilità: {corrispondente & #124; All}]|Specifica che i pacchetti per l'installazione se viene soddisfatto il criterio di filtro. **Corrispondente** significa installare solo i pacchetti driver che corrispondono a un hardware del client. **Tutti** significa installa tutti i pacchetti ai client indipendentemente dall'hardware.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per impostare le proprietà per un gruppo di driver, digitare uno dei seguenti:
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /Set-DriverGroup /DriverGroup:printerdrivers /Name:colorprinterdrivers /Applicability:All
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[sottocomando: set-DriverGroupFilter](subcommand-set-drivergroupfilter.md)
