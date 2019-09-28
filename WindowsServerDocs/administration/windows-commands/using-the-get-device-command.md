---
title: Utilizzando il comando get-dispositivo
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6451e0a55a72fc88867a3f3be0e1317d881391aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363203"
---
# <a name="using-the-get-device-command"></a>Utilizzando il comando get-dispositivo

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera le informazioni di servizi di distribuzione Windows su un computer pre-installato, ovvero un computer fisico che è stato allineato a un account computer in servizi di dominio Active Directory.
## <a name="syntax"></a>Sintassi
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/Device: <Device name>|Specifica il nome del computer (SAMAccountName).|
|/ID: <MAC or UUID>|Specifica l'indirizzo MAC o l'UUID (GUID) del computer, come illustrato negli esempi seguenti. Si noti che un GUID valido deve essere in uno dei due formati stringa binaria o stringa GUID<br /><br />**stringa binaria**-   :/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6<br />**indirizzo MAC**-   : 00B056882FDC (senza trattini) o 00-B0-56-88-2F-DC (con i trattini)<br />**stringa GUID**-   :/ID: E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/ Dominio:<Domain>]|Specifica il dominio in cui cercare il computer pre-installazione. Il valore predefinito per questo parametro è il dominio locale.|
|[/Forest: {Yes &#124; No}]|Specifica se servizi di distribuzione Windows deve eseguire la ricerca nell'intera foresta o nel dominio locale. Il valore predefinito è **No**, ovvero viene eseguita la ricerca solo nel dominio locale.|
## <a name="BKMK_examples"></a>Esempi
Per ottenere informazioni utilizzando il nome del computer, digitare:
```
wdsutil /Get-Device /Device:computer1
```
Per ottenere informazioni usando l'indirizzo MAC, digitare:
```
wdsutil /verbose /Get-Device /ID:"00-B0-56-88-2F-DC" /Domain:MyDomain
```
Per ottenere informazioni tramite la stringa GUID, digitare:
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[sottocomando: set-Device](subcommand-set-device.md)
[usando il comando Add-Device](using-the-add-device-command.md)
[usando il comando Get-AllDevices](using-the-get-alldevices-command.md)
