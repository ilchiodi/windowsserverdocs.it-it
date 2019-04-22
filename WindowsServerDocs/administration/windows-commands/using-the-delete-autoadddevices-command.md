---
title: Usando il comando delete AutoaddDevices
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b3375418c5ce0b02e187e292cac5b168f0de5dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813502"
---
# <a name="using-the-delete-autoadddevices-command"></a>Usando il comando delete AutoaddDevices

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina i computer che sono in sospeso, rifiutati o approvati dal database di aggiunta automatica. Questo database contiene informazioni su questi computer nel server.
## <a name="syntax"></a>Sintassi
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/Devicetype:{PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Specifica il tipo di computer da cui eliminare dal database. Può trattarsi di uno qualsiasi dei tre tipi seguenti:<br /><br />-   **PendingDevices** restituisce tutti i computer del database con stato in sospeso.<br />-   **RejectedDevices** restituisce tutti i computer nel database che have uno stato rifiutato.<br />-   **ApprovedDevices** restituisce tutti i computer con stato approvata.|
## <a name="BKMK_examples"></a>Esempi
Per eliminare rifiutati tutti i computer, digitare:
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Per eliminare approvati tutti i computer, digitare:
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[usando il comando Approva AutoaddDevices](using-the-approve-autoadddevices-command.md)
[utilizzando il comando get-AutoaddDevices](using-the-get-autoadddevices-command.md) 
 [Usando il comando AutoaddDevices rifiuto](using-the-reject-autoadddevices-command.md)
