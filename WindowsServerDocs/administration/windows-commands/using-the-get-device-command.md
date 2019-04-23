---
title: Utilizzando il comando get-dispositivo
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 117720c5102da5713c1b0bb5e59cbcc099f3aa8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871032"
---
# <a name="using-the-get-device-command"></a>Utilizzando il comando get-dispositivo

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera le informazioni di servizi di distribuzione di Windows su un computer pre-installazione (vale a dire, un computer fisico che è stato allineato a un account computer in servizi di dominio active directory.
## <a name="syntax"></a>Sintassi
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/ Dispositivo:<Device name>|Specifica il nome del computer (SAMAccountName).|
|/ID:<MAC or UUID>|Specifica l'indirizzo MAC o l'UUID (GUID) del computer, come illustrato negli esempi seguenti. Si noti che in uno dei due formati stringa binaria o una stringa GUID deve essere un GUID valido<br /><br />-   **Stringa binaria**: /ID:ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **Indirizzo MAC**: 00B056882FDC (senza trattini) o 00-B0-56-88-2F-DC (con i trattini)<br />-   **GUID string**: /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/ Dominio:<Domain>]|Specifica il dominio in cui cercare il computer pre-installazione. Il valore predefinito per questo parametro è il dominio locale.|
|[/ insieme di strutture: {Sì &#124; No}]|Specifica se i servizi di distribuzione Windows deve eseguire la ricerca nell'intera foresta o dominio locale. Il valore predefinito è **No**, vale a dire che solo il dominio locale viene eseguita una ricerca.|
## <a name="BKMK_examples"></a>Esempi
Per ottenere informazioni utilizzando il nome del computer, digitare:
```
wdsutil /Get-Device /Device:computer1
```
Per ottenere informazioni usando l'indirizzo MAC, digitare:
```
wdsutil /verbose /Get-Device /ID:"00-B0-56-88-2F-DC" /Domain:MyDomain
```
Per ottenere informazioni usando la stringa del GUID, digitare:
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[sottocomando: set-dispositivo](subcommand-set-device.md)
[utilizzando il comando Aggiungi dispositivo](using-the-add-device-command.md)
[tramite il Comando Get-AllDevices](using-the-get-alldevices-command.md)
