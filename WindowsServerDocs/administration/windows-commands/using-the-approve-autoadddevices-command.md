---
title: Usando il comando AutoaddDevices approvare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9ce9824c45a00ccb9f1f9e357c7e3d36b2857f69
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886822"
---
# <a name="using-the-approve-autoadddevices-command"></a>Usando il comando AutoaddDevices approvare

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Approva i computer che sono in attesa di approvazione dell'amministratore. Quando è abilitato il criterio di aggiunta automatica, approvazione dell'amministratore è necessaria prima di installano un'immagine ai computer sconosciuti (quelli che non sono pre-installati). È possibile abilitare questo criterio utilizzando il **risposta PXE** scheda della pagina delle proprietà server di s.
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
|[/OU:<DN of OU>]|Specifica il nome distinto dell'unità organizzativa (OU) in cui deve essere creato l'oggetto account computer. Ad esempio:  **OU = UnitàOrganizzativa, CN = Test, DC = Domain, DC = com**. Il percorso predefinito è il contenitore del computer predefinito.|
|[/ Utente: < dominio\utente &#124; User@Domain>]|Imposta le autorizzazioni sull'oggetto account computer per assegnare i diritti necessari all'utente specificato.|
|[/ JoinRights: {JoinOnly &#124; Completo}]|Specifica il tipo di diritti da assegnare all'utente specificato.<br /><br />-   **JoinOnly** richiede che l'amministratore reimpostare l'account del computer prima che l'utente può aggiungere il computer al dominio.<br />-   **Completa** fornisce l'accesso completo all'utente, che include il diritto di aggiungere il computer al dominio.|
|[/ JoinDomain: {Sì &#124; No}]|Specifica se il computer deve appartenere al dominio come account di questo computer durante l'installazione del sistema operativo. Il valore predefinito è **Sì**.|
|[/ ReferralServer:<Server name>]|Specifica il nome del server a cui essere contattati per scaricare l'immagine di avvio e di programma di avvio rete tramite Trivial File Transfer Protocol (tftp).|
|[/ BootProgram:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall per il programma di avvio di rete che deve ricevere questo computer. Ad esempio: **boot\x86\pxeboot.com**.|
|[/ WdsClientUnattend:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall del file di installazione automatica che consente di automatizzare il client di servizi di distribuzione Windows.|
|[/BootImagepath:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall all'immagine di avvio che deve ricevere questo computer.|
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
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[usando il comando delete AutoaddDevices](using-the-delete-autoadddevices-command.md)
[utilizzando il comando get-AutoaddDevices](using-the-get-autoadddevices-command.md) 
 [Usando il comando AutoaddDevices rifiuto](using-the-reject-autoadddevices-command.md)
