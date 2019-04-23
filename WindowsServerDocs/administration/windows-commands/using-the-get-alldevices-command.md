---
title: Utilizzando il comando get-AllDevices
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5f51bcc2332cced906be1eec3265541ffd2d225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886372"
---
# <a name="using-the-get-alldevices-command"></a>Utilizzando il comando get-AllDevices

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le proprietà di servizi di distribuzione Windows di tutti i computer pre-installazione. Un computer pre-installato è un computer fisico che è stato collegato a un account computer in servizi di dominio active directory.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/ insieme di strutture: {Sì &#124; No}]|Specifica se i servizi di distribuzione Windows deve restituire i computer nell'intero insieme di strutture o del dominio locale. L'impostazione predefinita è **n**, che significa che vengono restituiti solo i computer nel dominio locale.|
|[/ ReferralServer:<Server name>]|Restituisce solo i computer che pre-installazione per il server specificato.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare tutti i computer, digitare uno dei seguenti:
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[sottocomando: set-dispositivo](subcommand-set-device.md)
[utilizzando il comando Aggiungi dispositivo](using-the-add-device-command.md)
[utilizzando il comando get-dispositivo](using-the-get-device-command.md)
