---
title: Get-AllDevices
description: Argomento di riferimento per Get-AllDevices, che consente di visualizzare le proprietà di servizi di distribuzione Windows di tutti i computer pre-installazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 26e114be7ecf104687da237636b54b79e4114591
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720897"
---
# <a name="get-alldevices"></a>Get-AllDevices

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le proprietà di servizi di distribuzione Windows di tutti i computer pre-installazione. Un computer pre-installato è un computer fisico che è stato collegato a un account computer in servizi di dominio Active Directory.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Forest: {Yes &#124; No}]|Specifica se i servizi di distribuzione Windows deve restituire i computer nell'intero insieme di strutture o del dominio locale. L'impostazione predefinita è **n**, che significa che vengono restituiti solo i computer nel dominio locale.|
|[/ ReferralServer:<Server name>]|Restituisce solo i computer che pre-installazione per il server specificato.|
## <a name="examples"></a>Esempi
Per visualizzare tutti i computer, digitare uno dei seguenti:
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[sottocomando: Set-Device](subcommand-set-device.md)

[usando il comando Add-Device](using-the-add-device-command.md)[usando il comando Get-Device](using-the-get-device-command.md)
