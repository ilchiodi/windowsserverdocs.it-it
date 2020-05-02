---
title: Reject-AutoaddDevices
description: Argomento di riferimento per Reject-AutoaddDevices, che rifiuta i computer in attesa di approvazione amministrativa.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e377d4e2d4aecea2e0ba3af023af39ab7695c0a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725919"
---
# <a name="reject-autoadddevices"></a>Reject-AutoaddDevices

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rifiuta i computer in attesa di approvazione amministrativa. Quando il criterio di aggiunta automatica è abilitato, è necessaria l'approvazione amministrativa prima che i computer sconosciuti (quelli che non sono pre-installati) possano installare un'immagine. È possibile abilitare questo criterio utilizzando la scheda **risposta PXE** della pagina delle proprietà del server.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/RequestId: ID richiesta <&#124; tutti>|Specifica l'ID richiesta assegnato al computer in sospeso. Per rifiutare tutti i computer in sospeso, specificare **All**.|
## <a name="examples"></a>Esempi
Per rifiutare un singolo computer, digitare:
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
Per rifiutare tutti i computer, digitare:
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando approva-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[utilizzando il comando Delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[utilizzando il comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
