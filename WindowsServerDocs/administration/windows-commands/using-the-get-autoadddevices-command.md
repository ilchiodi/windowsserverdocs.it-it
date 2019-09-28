---
title: Utilizzando il comando Get-AutoaddDevices
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fa30a8ebd73164dc3d8ab267c3deb0739aa4b700
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363255"
---
# <a name="using-the-get-autoadddevices-command"></a>Utilizzando il comando Get-AutoaddDevices

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza tutti i computer presenti nel database di aggiunta automatica in un server di servizi di distribuzione Windows.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|Specifica il tipo di computer da restituire.<br /><br />-   **PendingDevices** restituisce tutti i computer del database il cui stato è in sospeso.<br />-   **RejectedDevices** restituisce tutti i computer del database che hanno lo stato rifiutato.<br />-   **ApprovedDevices** restituisce tutti i computer del database che hanno lo stato approvato.|
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
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
 usando il comando[delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
 usando il comando[approva-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[usando il comando REJECT-AutoaddDevices](using-the-reject-autoadddevices-command.md)
