---
title: Uso del comando REJECT-AutoaddDevices
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e8fda3037ef921e2b2a7a0acb616b8a67545ff9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362998"
---
# <a name="using-the-reject-autoadddevices-command"></a>Uso del comando REJECT-AutoaddDevices

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rifiuta i computer in attesa di approvazione amministrativa. Quando il criterio di aggiunta automatica è abilitato, è necessaria l'approvazione amministrativa prima che i computer sconosciuti (quelli che non sono pre-installati) possano installare un'immagine. È possibile abilitare questo criterio utilizzando la scheda **risposta PXE** della pagina delle proprietà del server.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/RequestId: ID &#124; richiesta < tutti >|Specifica l'ID richiesta assegnato al computer in sospeso. Per rifiutare tutti i computer in sospeso, specificare **All**.|
## <a name="BKMK_examples"></a>Esempi
Per rifiutare un singolo computer, digitare:
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
Per rifiutare tutti i computer, digitare:
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
 usando il comando[approva-AutoaddDevices](using-the-approve-autoadddevices-command.md)
 usando il comando[Delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[usando il comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
