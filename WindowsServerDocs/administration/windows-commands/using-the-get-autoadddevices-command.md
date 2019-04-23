---
title: Utilizzando il comando get-AutoaddDevices
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 337c8e76923fe243982ba9c10d18f2e5a5e7d9ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885742"
---
# <a name="using-the-get-autoadddevices-command"></a>Utilizzando il comando get-AutoaddDevices

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza tutti i computer che sono nel database di aggiunta automatica in un server di servizi di distribuzione Windows.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/Devicetype:{PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|Specifica il tipo di computer da restituire.<br /><br />-   **PendingDevices** restituisce tutti i computer del database con stato in sospeso.<br />-   **RejectedDevices** restituisce tutti i computer nel database che have uno stato rifiutato.<br />-   **ApprovedDevices** restituisce tutti i computer nel database che have lo stato approvato.|
## <a name="BKMK_examples"></a>Esempi
Per visualizzare tutti i computer approvati, digitare:
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
Per visualizzare tutti i computer rifiutati, digitare:
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[usando il comando delete AutoaddDevices](using-the-delete-autoadddevices-command.md)
[utilizzando il comando Approva AutoaddDevices](using-the-approve-autoadddevices-command.md) 
 [ Usando il comando AutoaddDevices rifiuto](using-the-reject-autoadddevices-command.md)
