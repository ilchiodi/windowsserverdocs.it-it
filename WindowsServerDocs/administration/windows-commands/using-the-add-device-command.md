---
title: Aggiungi dispositivo
description: Argomento dei comandi di Windows per Add-Device, che pre-installa un computer in servizi di dominio Active Directory. Computer pre-installazione sono detti anche computer noti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e599cc4-464a-421b-b6bb-c101af154131
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0acccb75d5e378a556e4b7ca8b37f805a8743dae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832164"
---
# <a name="add-device"></a>Aggiungi dispositivo

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pre-installazione di un computer in servizi di dominio Active Directory. Computer pre-installazione sono detti anche computer noti. Ciò consente di configurare le proprietà per controllare l'installazione del client. Ad esempio, è possibile configurare il programma di avvio di rete e il file di installazione automatica che il client deve ricevere, nonché il server da cui il client deve scaricare il programma di avvio di rete.
per esempi di come è possibile usare questo comando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi
```
wdsutil /add-Device /Device:<Device name> /ID:<UUID | MAC address> [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] [/WdsClientUnattend:<Relative path>] 
[/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/OU:<DN of OU>] [/Domain:<Domain>]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/Device:<computer name>|Specifica il nome del computer da aggiungere.|
|/ ID: < UUID &#124; indirizzo MAC >|Specifica il GUID/UUID o l'indirizzo MAC del computer. Un GUID/UUID deve trovarsi in uno dei due formati stringa binaria o stringa GUID. Ad esempio,<p>Stringa binaria: **/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6**<p>Stringa GUID: **/ID:E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<p>Deve essere un indirizzo MAC nel formato seguente: **00B056882FDC** (senza trattini) o **00-B0-56-88-2F-DC** (con i trattini)|
|[/ ReferralServer:<Server name>]|Specifica il nome del server da contattare per scaricare il programma di avvio di rete e l'immagine di avvio utilizzando Trivial File Transfer Protocol (TFTP).|
|[/ BootProgram:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall al programma di avvio di rete che deve ricevere il computer. Ad esempio: boot\x86\pxeboot.com|
|[/ WdsClientUnattend:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall al file di installazione automatica che consente di automatizzare le schermate di installazione del client di servizi di distribuzione Windows.|
|[/User: < dominio\utente &#124; User@Domain>]|Imposta le autorizzazioni sull'oggetto account computer per consentire all'utente specificato i diritti necessari per aggiungere il computer al dominio.|
|[/ JoinRights: {JoinOnly & #124; Completo}]|Specifica il tipo di diritti da assegnare all'utente.<p>-   **JoinOnly** richiede all'amministratore di reimpostare l'account del computer prima che l'utente possa aggiungere il computer al dominio.<br />-   **completa** fornisce l'accesso completo all'utente, che include il diritto di aggiungere il computer al dominio.|
|[/ JoinDomain: {Sì & #124; No}]|Specifica se il computer deve appartenere al dominio come account di questo computer durante l'installazione del sistema operativo. Il valore predefinito è **Sì**.|
|[/BootImagepath:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall all'immagine di avvio che deve essere utilizzata dal computer.|
|[/OU:<DN of OU>]|Il nome distinto dell'unità organizzativa in cui deve essere creato l'oggetto account computer. Ad esempio: **OU = UnitàOrganizzativa, CN = Test, DC = Domain, DC = com**. Il percorso predefinito è il contenitore del computer predefinito.|
|[/ Dominio:<Domain>]|Il dominio in cui deve essere creato l'oggetto account computer. Il percorso predefinito è il dominio locale.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per aggiungere un computer utilizzando un indirizzo MAC, digitare:
```
wdsutil /add-Device /Device:computer1 /ID:00-B0-56-88-2F-DC
```
Per aggiungere un computer con una stringa GUID, digitare:
```
wdsutil /add-Device /Device:computer1 /ID:{E8A3EFAC-201F-4E69-953F-B2DAA1E8B1B6} /ReferralServer:WDSServer1 /BootProgram:boot\x86\pxeboot.com 
/WDSClientUnattend:WDSClientUnattend\unattend.xml /User:Domain\MyUser/JoinRights:Full /BootImagepath:boot\x86\images\boot.wim /OU:OU=MyOU,CN=Test,DC=Domain,DC=com
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[utilizzando il comando get-AllDevices](using-the-get-alldevices-command.md)
[utilizzando il comando get-dispositivo](using-the-get-device-command.md)
[sottocomando: set-dispositivo](subcommand-set-device.md)
[WdsClient New](https://technet.microsoft.com/library/dn283430.aspx)
