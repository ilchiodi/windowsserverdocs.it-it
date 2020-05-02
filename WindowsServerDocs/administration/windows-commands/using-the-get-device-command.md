---
title: Get-Device
description: Argomento di riferimento per Get-Device, che recupera le informazioni di servizi di distribuzione Windows su un computer pre-installato, ovvero un computer fisico che è stato allineato a un account computer in servizi di dominio Active Directory.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f89266a2f70523ec332ed7cfb6a976f87a8e4f2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719963"
---
# <a name="get-device"></a>Get-Device

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera le informazioni di servizi di distribuzione Windows su un computer pre-installato, ovvero un computer fisico che è stato allineato a un account computer in servizi di dominio Active Directory.

## <a name="syntax"></a>Sintassi
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|Dispositivo<Device name>|Specifica il nome del computer (SAMAccountName).|
|ID<MAC or UUID>|Specifica l'indirizzo MAC o l'UUID (GUID) del computer, come illustrato negli esempi seguenti. Si noti che un GUID valido deve essere in uno dei due formati stringa binaria o stringa GUID<p>-   **Stringa binaria**:/ID: ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **Indirizzo Mac**: 00B056882FDC (senza trattini) o 00-B0-56-88-2F-DC (con i trattini)<br />-   **Stringa GUID**:/ID: E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/ Dominio:<Domain>]|Specifica il dominio in cui cercare il computer pre-installazione. Il valore predefinito per questo parametro è il dominio locale.|
|[/Forest: {Yes &#124; No}]|Specifica se servizi di distribuzione Windows deve eseguire la ricerca nell'intera foresta o nel dominio locale. Il valore predefinito è **No**, ovvero viene eseguita la ricerca solo nel dominio locale.|
## <a name="examples"></a>Esempi
Per ottenere informazioni utilizzando il nome del computer, digitare:
```
wdsutil /Get-Device /Device:computer1
```
Per ottenere informazioni usando l'indirizzo MAC, digitare:
```
wdsutil /verbose /Get-Device /ID:00-B0-56-88-2F-DC /Domain:MyDomain
```
Per ottenere informazioni tramite la stringa GUID, digitare:
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[sottocomando: Set-Device](subcommand-set-device.md)

[usando il comando Add-Device](using-the-add-device-command.md)[usando il comando Get-AllDevices](using-the-get-alldevices-command.md)
