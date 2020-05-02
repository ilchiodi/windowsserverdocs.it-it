---
title: Get-AutoaddDevices
description: Argomento di riferimento per Get-AutoaddDevices, che consente di visualizzare tutti i computer presenti nel database di aggiunta automatica in un server di servizi di distribuzione Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c15836fa81c694aa9295d0a98376f4bef3125243
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719995"
---
# <a name="get-autoadddevices"></a>Get-AutoaddDevices

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza tutti i computer presenti nel database di aggiunta automatica in un server di servizi di distribuzione Windows.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|Specifica il tipo di computer da restituire.<p>-   **PendingDevices** restituisce tutti i computer del database che hanno lo stato in sospeso.<br />-   **RejectedDevices** restituisce tutti i computer del database che hanno lo stato rifiutato.<br />-   **ApprovedDevices** restituisce tutti i computer del database che hanno lo stato approvato.|
## <a name="examples"></a>Esempi
Per visualizzare tutti i computer approvati, digitare:
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
Per visualizzare tutti i computer rifiutati, digitare:
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Chiave](command-line-syntax-key.md)
di sintassi della riga di comando usando il comando[Delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[usando il comando approva-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[usando il comando REJECT-AutoaddDevices](using-the-reject-autoadddevices-command.md)
