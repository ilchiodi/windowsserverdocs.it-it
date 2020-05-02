---
title: Delete-AutoaddDevices
description: Argomento di riferimento per Delete-AutoaddDevices, che elimina i computer in sospeso, rifiutati o approvati dal database di aggiunta automatica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 90b5b24b68b2cfe3d387cb02b3715b70edba4300
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720986"
---
# <a name="delete-autoadddevices"></a>Delete-AutoaddDevices

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina i computer in sospeso, rifiutati o approvati dal database di aggiunta automatica. Questo database contiene informazioni su questi computer nel server.

## <a name="syntax"></a>Sintassi
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Specifica il tipo di computer da cui eliminare dal database. Può trattarsi di uno qualsiasi dei tre tipi seguenti:<p>-   **PendingDevices** restituisce tutti i computer del database che hanno lo stato in sospeso.<br />-   **RejectedDevices** restituisce tutti i computer del database che hanno lo stato rifiutato.<br />-   **ApprovedDevices** restituisce tutti i computer con stato approvato.|
## <a name="examples"></a>Esempi
Per eliminare rifiutati tutti i computer, digitare:
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Per eliminare approvati tutti i computer, digitare:
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
di sintassi della riga di comando usando il comando[approva-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[usando il comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
[usando il comando REJECT-AutoaddDevices](using-the-reject-autoadddevices-command.md)
