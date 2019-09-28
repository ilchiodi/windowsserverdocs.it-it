---
title: Utilizzando il comando get-AllDevices
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5b7d2ce709c7e3fbaf7ab4f0e49be14c98ba1cd9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401976"
---
# <a name="using-the-get-alldevices-command"></a>Utilizzando il comando get-AllDevices

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza le proprietà di servizi di distribuzione Windows di tutti i computer pre-installazione. Un computer pre-installato è un computer fisico che è stato collegato a un account computer in servizi di dominio Active Directory.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Forest: {Yes &#124; No}]|Specifica se i servizi di distribuzione Windows deve restituire i computer nell'intero insieme di strutture o del dominio locale. L'impostazione predefinita è **n**, che significa che vengono restituiti solo i computer nel dominio locale.|
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
