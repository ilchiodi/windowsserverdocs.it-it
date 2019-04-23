---
title: Usando il comando AutoaddDevices rifiuto
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: af46aec7c8f02b3600983b66bd1b0ac6f5dd1dcc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852562"
---
# <a name="using-the-reject-autoadddevices-command"></a>Usando il comando AutoaddDevices rifiuto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rifiuta i computer che sono in attesa di approvazione dell'amministratore. Quando è abilitato il criterio di aggiunta automatica, approvazione dell'amministratore è necessaria prima di installano un'immagine ai computer sconosciuti (quelli che non sono pre-installati). È possibile abilitare questo criterio utilizzando il **risposta PXE** scheda della pagina delle proprietà server di s.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/RequestId:<Request ID &#124; ALL>|Specifica l'ID richiesta assegnato al computer in sospeso. Per rifiutare tutti i computer in sospeso, specificare **tutti**.|
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
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[usando il comando Approva AutoaddDevices](using-the-approve-autoadddevices-command.md)
[utilizzando il comando delete AutoaddDevices](using-the-delete-autoadddevices-command.md) 
 [ Utilizzando il comando get-AutoaddDevices](using-the-get-autoadddevices-command.md)
