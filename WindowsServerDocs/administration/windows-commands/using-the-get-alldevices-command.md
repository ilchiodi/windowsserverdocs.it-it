---
title: Get-AllDevices
description: Windows Commands Topic for Get-AllDevices, che visualizza le proprietà di servizi di distribuzione Windows di tutti i computer pre-installati.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 929a74b6cccaed6e85015648538c1ca875b62208
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831414"
---
# <a name="get-alldevices"></a>Get-AllDevices

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per visualizzare tutti i computer, digitare uno dei seguenti:
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[sottocomando: set-dispositivo](subcommand-set-device.md)
[utilizzando il comando Aggiungi dispositivo](using-the-add-device-command.md)
[utilizzando il comando get-dispositivo](using-the-get-device-command.md)
