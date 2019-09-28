---
title: Utilizzando il comando approva-AutoaddDevices
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d76e8d3-ab35-429c-be7b-904f95d0782d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5617837706a778bac2456647f40efd563d861722
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363660"
---
# <a name="using-the-approve-autoadddevices-command"></a>Utilizzando il comando approva-AutoaddDevices

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Approva i computer che sono in attesa di approvazione dell'amministratore. Quando il criterio di aggiunta automatica è abilitato, è necessaria l'approvazione amministrativa prima che i computer sconosciuti (quelli che non sono pre-installati) possano installare un'immagine. È possibile abilitare questo criterio utilizzando la scheda **risposta PXE** della pagina delle proprietà del server.
## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Approve-AutoaddDevices [/Server:<Server name>] /RequestId:{<Request ID>| ALL} [/MachineName:<Device name>] [/OU:<DN of OU>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] [/BootImagepath:<Relative path>]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del server. Può essere il nome NetBIOS oppure il nome di dominio completo. Se non si specifica alcun nome server, verrà utilizzato il server locale.|
|/ RequestId: {richiesta ID &#124; ALL}|Specifica l'ID richiesta assegnato al computer in sospeso. Specificare **TUTTI** per approvare tutti i computer in sospeso.|
|[/ NomeComputer:<Device name>]|Specifica il nome del computer da aggiungere. È possibile utilizzare questa opzione quando si approvano tutti i computer.|
|[/OU:<DN of OU>]|Specifica il nome distinto dell'unità organizzativa (OU) in cui deve essere creato l'oggetto account computer. Ad esempio:  **OU = UnitàOrganizzativa, CN = test, DC = Domain, DC = com**. Il percorso predefinito è il contenitore del computer predefinito.|
|[/ Utente: < dominio\utente &#124; User@Domain>]|Imposta le autorizzazioni sull'oggetto account computer per assegnare i diritti necessari all'utente specificato.|
|[/ JoinRights: {JoinOnly &#124; Completo}]|Specifica il tipo di diritti da assegnare all'utente specificato.<br /><br />-   **JoinOnly** richiede all'amministratore di reimpostare l'account del computer prima che l'utente possa aggiungere il computer al dominio.<br />-   **full** fornisce l'accesso completo all'utente, che include il diritto di aggiungere il computer al dominio.|
|[/ JoinDomain: {Sì &#124; No}]|Specifica se il computer deve appartenere al dominio come account di questo computer durante l'installazione del sistema operativo. Il valore predefinito è **Sì**.|
|[/ ReferralServer:<Server name>]|Specifica il nome del server da contattare per scaricare il programma di avvio e l'immagine di avvio di rete utilizzando Trivial File Transfer Protocol (TFTP).|
|[/ BootProgram:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall al programma di avvio di rete che deve ricevere il computer. Ad esempio: **boot\x86\pxeboot.com**.|
|[/ WdsClientUnattend:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall al file di installazione automatica che consente di automatizzare il client di servizi di distribuzione Windows.|
|[/BootImagepath:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall all'immagine di avvio che questo computer deve ricevere.|
## <a name="BKMK_examples"></a>Esempi
Per approvare il computer con un ID di 12, digitare:
```
wdsutil /Approve-AutoaddDevices /RequestId:12
```
Per approvare il computer con un ID di 20 e distribuire l'immagine con le impostazioni specificate, digitare:
```
wdsutil /Approve-AutoaddDevices /RequestId:20 /MachineName:computer1 /OU:"OU=Test,CN=company,DC=Domain,DC=Com" /User:Domain\User1 
/JoinRights:Full /ReferralServer:MyWDSServer /BootProgram:boot\x86\pxeboot.n12 /WdsClientUnattend:WDSClientUnattend\Unattend.xml /BootImagepath:boot\x86\images\boot.wim
```
Per approvare tutti i computer in sospeso, digitare:
```
wdsutil /verbose /Approve-AutoaddDevices /RequestId:ALL
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave della sintassi della riga di comando](command-line-syntax-key.md)
 usando il comando[delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
 usando il comando[Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
[usando il comando REJECT-AutoaddDevices](using-the-reject-autoadddevices-command.md)
