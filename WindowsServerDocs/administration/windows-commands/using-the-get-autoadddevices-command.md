---
title: Get-AutoaddDevices
description: Windows Commands Topic for Get-AutoaddDevices, che consente di visualizzare tutti i computer presenti nel database di aggiunta automatica in un server di servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b373fca81769ff1296d0e9a0788fe536c4fc3132
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831184"
---
# <a name="get-autoadddevices"></a>Get-AutoaddDevices

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza tutti i computer presenti nel database di aggiunta automatica in un server di servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|Specifica il tipo di computer da restituire.<p>-   **PendingDevices** restituisce tutti i computer del database il cui stato è in sospeso.<br />-   **RejectedDevices** restituisce tutti i computer del database che hanno lo stato rifiutato.<br />-   **ApprovedDevices** restituisce tutti i computer del database che hanno lo stato approvato.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per visualizzare tutti i computer approvati, digitare:
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
Per visualizzare tutti i computer rifiutati, digitare:
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
usando il comando [delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
usando il comando [approva-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[usando il comando REJECT-AutoaddDevices](using-the-reject-autoadddevices-command.md)
