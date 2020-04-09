---
title: Reject-AutoaddDevices
description: Windows Commands argomento per Reject-AutoaddDevices, che rifiuta i computer in attesa di approvazione amministrativa.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39bdddbbc169a0a0810fcc67e95224360858b728
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830654"
---
# <a name="reject-autoadddevices"></a>Reject-AutoaddDevices

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rifiuta i computer in attesa di approvazione amministrativa. Quando il criterio di aggiunta automatica è abilitato, è necessaria l'approvazione amministrativa prima che i computer sconosciuti (quelli che non sono pre-installati) possano installare un'immagine. È possibile abilitare questo criterio utilizzando la scheda **risposta PXE** della pagina delle proprietà del server.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/RequestId: ID &#124; richiesta < tutti >|Specifica l'ID richiesta assegnato al computer in sospeso. Per rifiutare tutti i computer in sospeso, specificare **All**.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per rifiutare un singolo computer, digitare:
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
Per rifiutare tutti i computer, digitare:
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)
usando il comando [approva-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[usando il comando Delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[usando il comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
