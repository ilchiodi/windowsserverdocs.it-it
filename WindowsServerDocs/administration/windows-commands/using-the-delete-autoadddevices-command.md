---
title: Delete-AutoaddDevices
description: Windows Commands Topic for delete-AutoaddDevices, che elimina i computer in sospeso, rifiutati o approvati dal database di aggiunta automatica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29df0bd92859e9ee0b5b5bedfbd2e66173059cb5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831664"
---
# <a name="delete-autoadddevices"></a>Delete-AutoaddDevices

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina i computer in sospeso, rifiutati o approvati dal database di aggiunta automatica. Questo database contiene informazioni su questi computer nel server.

## <a name="syntax"></a>Sintassi
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Specifica il tipo di computer da cui eliminare dal database. Può trattarsi di uno qualsiasi dei tre tipi seguenti:<p>-   **PendingDevices** restituisce tutti i computer del database il cui stato è in sospeso.<br />-   **RejectedDevices** restituisce tutti i computer del database che hanno lo stato rifiutato.<br />-   **ApprovedDevices** restituisce tutti i computer con stato approvato.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per eliminare rifiutati tutti i computer, digitare:
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Per eliminare approvati tutti i computer, digitare:
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
usando il comando [approva-AutoaddDevices](using-the-approve-autoadddevices-command.md)
usando il comando [Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
[usando il comando REJECT-AutoaddDevices](using-the-reject-autoadddevices-command.md)
