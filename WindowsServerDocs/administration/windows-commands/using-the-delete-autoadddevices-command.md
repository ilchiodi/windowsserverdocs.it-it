---
title: Uso del comando Delete-AutoaddDevices
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e914506f731685b17117b359359f60ea91992dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363540"
---
# <a name="using-the-delete-autoadddevices-command"></a>Uso del comando Delete-AutoaddDevices

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Elimina i computer in sospeso, rifiutati o approvati dal database di aggiunta automatica. Questo database contiene informazioni su questi computer nel server.
## <a name="syntax"></a>Sintassi
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se viene specificato alcun nome di server, verrà utilizzato il server locale.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Specifica il tipo di computer da cui eliminare dal database. Può trattarsi di uno qualsiasi dei tre tipi seguenti:<br /><br />-   **PendingDevices** restituisce tutti i computer del database il cui stato è in sospeso.<br />-   **RejectedDevices** restituisce tutti i computer del database che hanno lo stato rifiutato.<br />-   **ApprovedDevices** restituisce tutti i computer con stato approvato.|
## <a name="BKMK_examples"></a>Esempi
Per eliminare rifiutati tutti i computer, digitare:
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Per eliminare approvati tutti i computer, digitare:
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
 usando il comando[approva-AutoaddDevices](using-the-approve-autoadddevices-command.md)
 usando il comando[Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
[usando il comando REJECT-AutoaddDevices](using-the-reject-autoadddevices-command.md)
