---
title: Set di sottocomandi-dispositivo
description: Argomento dei comandi di Windows per il sottocomando set-Device, che modifica gli attributi di un computer pre-installazione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 401567f8-eaeb-4a2d-b811-140bb007028d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ced5f44c9edda0176c93479ded60d809644bf4fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834044"
---
# <a name="subcommand-set-device"></a>Sottocomando: set-dispositivo

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modificare gli attributi di un computer pre-installazione. Un computer pre-installato è un computer collegato a un oggetto account computer in Active Directory Domain Servers (AD DS). Client pre-installazione sono detti anche computer noti. È possibile configurare le proprietà dell'account computer per controllare l'installazione del client. Ad esempio, è possibile configurare il programma di avvio di rete e il file di installazione automatica che il client deve ricevere, nonché il server da cui il client deve scaricare il programma di avvio di rete.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Set-Device /Device:<Device name> [/ID:<UUID | MAC address>] [/ReferralServer:<Server name>] [/BootProgram:<Relative path>] 
[/WdsClientUnattend:<Relative path>] [/User:<Domain\User | User@Domain>] [/JoinRights:{JoinOnly | Full}] [/JoinDomain:{Yes | No}] [/BootImagepath:<Relative path>] [/Domain:<Domain>] [/resetAccount]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/Device:<computer name>|Specifica il nome del computer (nome Account SAM).|
|[/ ID: < UUID & #124; Indirizzo MAC >]|Specifica il GUID/UUID o l'indirizzo MAC del computer. Questo valore deve essere in uno dei tre formati seguenti:<p>-Stringa binaria: **/ID:ACEFA3E81F20694E953EB2DAA1E8B1B6**<br />-Stringa GUID/UUID: / ID:**E8A3EFAC-201F-4E69-953E-B2DAA1E8B1B6**<br />-Indirizzo MAC: **00B056882FDC** (senza trattini) o **00-B0-56-88-2F-DC** (con i trattini)|
|[/ ReferralServer:<Server name>]|Specifica il nome del server da contattare per scaricare il programma di avvio e l'immagine di avvio di rete utilizzando Trivial File Transfer Protocol (TFTP).|
|[/ BootProgram:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall al programma di avvio di rete che riceverà il computer specificato. Ad esempio: **boot\x86\pxeboot.com**|
|[/ WdsClientUnattend:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall al file di installazione automatica che consente di automatizzare le schermate di installazione per il client di servizi di distribuzione Windows.|
|[/User: < dominio\utente &#124; User@Domain>]|Imposta le autorizzazioni sull'oggetto account computer per consentire all'utente specificato i diritti necessari per aggiungere il computer al dominio.|
|[/ JoinRights: {JoinOnly & #124; Completo}]|Specifica il tipo di diritti da assegnare all'utente.<p>-   **JoinOnly** richiede all'amministratore di reimpostare l'account del computer prima che l'utente possa aggiungere il computer al dominio.<br />-   **completa** fornisce l'accesso completo all'utente, incluso il diritto di aggiungere il computer al dominio.|
|[/ JoinDomain: {Sì & #124; No}]|Specifica se il computer deve appartenere al dominio come account di questo computer durante un'installazione di servizi di distribuzione Windows. L'impostazione predefinita è **Sì**.|
|[/BootImagepath:<Relative path>]|Specifica il percorso relativo dalla cartella remoteInstall all'immagine di avvio che il computer utilizzerà.|
|[/ Dominio:<Domain>]|Specifica il dominio in cui cercare il computer pre-installazione. Il valore predefinito è il dominio locale.|
|/ResetAccount|Reimposta le autorizzazioni nel computer specificato in modo che chiunque disponga delle autorizzazioni appropriate possa accedere al dominio utilizzando questo account.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per impostare server programma e di riferimento di avvio di rete per un computer, digitare:
```
wdsutil /Set-Device /Device:computer1 /ReferralServer:MyWDSServer
/BootProgram:boot\x86\pxeboot.n12
```
Per impostare varie impostazioni per un computer, digitare:
```
wdsutil /verbose /Set-Device /Device:computer2 /ID:00-B0-56-88-2F-DC /WdsClientUnattend:WDSClientUnattend\unattend.xml 
/User:Domain\user /JoinRights:JoinOnly /JoinDomain:No /BootImagepath:boot\x86\images\boot.wim /Domain:NorthAmerica /resetAccount
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando Aggiungi dispositivo](using-the-add-device-command.md)
[utilizzando il comando get-AllDevices](using-the-get-alldevices-command.md)
[utilizzando il comando get-dispositivo](using-the-get-device-command.md)
