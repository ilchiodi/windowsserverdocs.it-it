---
ms.assetid: ''
title: I tipi in ADFS di attestazione di accesso client
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1e37aded450555d293806d1ed8903a51e3df9424
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839142"
---
#<a name="client-access-policy-claim-types-in-ad-fs"></a>Criterio di accesso client di attestazione tipi in AD FS

Per fornire informazioni sul contesto di richiesta aggiuntivi, i criteri di accesso Client utilizzare i seguenti tipi di attestazione, ADFS genera dalle informazioni di intestazione di richiesta per l'elaborazione.  Per altre informazioni, vedere [il ruolo del motore di attestazioni](../technical-reference/the-role-of-the-claims-engine.md).

##<a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

Questa attestazione AD FS rappresenta un "miglior tentativo possibile" di verificare l'indirizzo IP dell'utente (ad esempio, il client di Outlook) effettua la richiesta. Questa attestazione può contenere più indirizzi IP, incluso l'indirizzo di ogni proxy che inoltrato la richiesta.  Questa attestazione viene popolata con un'intestazione HTTP che è attualmente impostato solo per Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per AD FS. Il valore dell'attestazione può essere uno dei seguenti:


- Un singolo indirizzo IP: indirizzo IP del client che è direttamente connesso a Exchange Online

    >! [Nota] L'indirizzo IP di un client nella rete aziendale verrà visualizzato come indirizzo IP interfaccia esterna del proxy in uscita dall'organizzazione o un gateway.

- Uno o più indirizzi IP
    - Se Exchange Online non è possibile determinare l'indirizzo IP del client che si connette, imposta il valore in base al valore dell'intestazione x-forwarded-for, un'intestazione non standard che può essere inclusi in basato su HTTP richiede ed è supportata da molti client, servizi di bilanciamento del carico e proxy sul mercato.
    - Più indirizzi IP che indica l'indirizzo di ogni proxy che trasmesso la richiesta e l'indirizzo IP del client saranno separati da una virgola.

    >! [Nota] Gli indirizzi IP correlati all'infrastruttura Exchange Online non saranno presenti nell'elenco.


>! [Avviso] Exchange Online supporta attualmente solo gli indirizzi IPV4. non supporta gli indirizzi IPV6. 


## <a name="x-ms-client-application"></a>X-MS-Client-Application

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

Questa attestazione AD FS rappresenta il protocollo usato dal client finale, che corrisponde a regime di controllo libero per l'applicazione in uso.  Questa attestazione viene popolata con un'intestazione HTTP che è attualmente impostato solo per Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per AD FS. A seconda dell'applicazione, il valore di questa attestazione sarà uno dei seguenti:



- Nel caso dei dispositivi che usano Exchange Active Sync, il valore è Microsoft.Exchange.ActiveSync. 
- Utilizzo del client Microsoft Outlook può comportare uno qualsiasi dei valori seguenti:
    - Microsoft.Exchange.Autodiscover
    - Microsoft.Exchange.OfflineAddressBook
    - Microsoft.Exchange.RPC
    - Microsoft.Exchange.WebServices
    - Microsoft.Exchange.Mapi
- Altri valori possibili per questa intestazione includono quanto segue:
    - Microsoft.Exchange.Powershell
    - Microsoft.Exchange.SMTP
    - Microsoft.Exchange.PopImap
    - Microsoft.Exchange.Pop
    - Microsoft.Exchange.Imap

## <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

Questa attestazione AD FS fornisce una stringa che rappresenti il tipo di dispositivo che il client usa per accedere al servizio. Può essere utilizzato quando i clienti desiderano impedire l'accesso per alcuni dispositivi (ad esempio particolari tipi di Smartphone).  Questa attestazione viene popolata con un'intestazione HTTP che è attualmente impostato solo per Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per AD FS. I valori di esempio per questa attestazione includono (ma non sono limitati a) i valori riportati di seguito.
>! [Nota] Di seguito sono riportati esempi di ciò che potrebbe contenere il valore di x-ms-user-agent per un client con cui x-ms-client-application "Microsoft.Exchange.ActiveSync"

- Vortex/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0.3

>! [Nota] È anche possibile che questo valore è vuoto.


## <a name="x-ms-proxy"></a>X-MS-Proxy

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

Questa attestazione AD FS indica che la richiesta è passata attraverso il proxy server federativo.  Questa attestazione viene popolata dai proxy server federativo, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per il back-end del servizio federativo. ADFS viene poi convertita in un'attestazione. 

Il valore dell'attestazione è il nome DNS del proxy server federativo che trasmesso la richiesta.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-Absolute-Path (attivo o passivo)

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Questo tipo di attestazione può essere utilizzato per determinare le richieste provenienti dai client (avanzate) "attivo" e "passivo" client (web basate su browser). In questo modo le richieste esterne da applicazioni basate su browser come Outlook Web Access, SharePoint Online o il portale di Office 365 deve essere autorizzato mentre vengono bloccate le richieste provenienti da rich client, ad esempio Microsoft Outlook.

Il valore dell'attestazione è il nome del servizio AD FS che ha ricevuto la richiesta.
