---
ms.assetid: 
title: I tipi in ADFS di attestazione di accesso client
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1e37aded450555d293806d1ed8903a51e3df9424
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
#<a name="client-access-policy-claim-types-in-ad-fs"></a>Criteri di accesso client di attestazione tipi in ADFS

Per fornire informazioni di contesto richiesta aggiuntiva, i criteri di accesso Client utilizzare i seguenti tipi di attestazione, ADFS genera dalle informazioni di intestazione di richiesta per l'elaborazione.  Per ulteriori informazioni vedere [il ruolo del motore di attestazioni di](../technical-reference/the-role-of-the-claims-engine.md).

##<a name="x-ms-forwarded-client-ip"></a>X-MS-inoltrati-Client-IP

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

L'attestazione ADFS rappresenta un "migliore tentativo di" accertare l'indirizzo IP dell'utente (ad esempio, il client Outlook) effettua la richiesta. L'attestazione può contenere più indirizzi IP, inclusi l'indirizzo di ogni proxy inoltrato la richiesta.  L'attestazione viene popolata con un'intestazione HTTP attualmente impostata solo per Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per ADFS. Il valore dell'attestazione può essere uno dei seguenti:


- Un singolo indirizzo IP - l'indirizzo IP del client connessi direttamente a Exchange Online

    >! [Nota] L'indirizzo IP di un client nella rete aziendale verrà visualizzato come l'indirizzo IP di interfaccia esterna del proxy in uscita dell'organizzazione o un gateway.

- Uno o più indirizzi IP
    - Se Exchange Online non può determinare l'indirizzo IP del client, imposterà il valore in base al valore dell'intestazione x-inoltrati-per un'intestazione non standard che può essere inclusi in basate su HTTP richieste ed è supportata da molti client, i servizi di bilanciamento del carico e proxy sul mercato.
    - Più indirizzi IP che indica l'indirizzo IP del client e l'indirizzo di ciascun proxy che ha trasmesso la richiesta saranno separati da una virgola.

    >! [Nota] Gli indirizzi IP relativi all'infrastruttura Exchange Online non sarà presenti nell'elenco.


>! [Avviso] Exchange Online attualmente supporta solo indirizzi IPV4. non supporta gli indirizzi IPV6. 


## <a name="x-ms-client-application"></a>Applicazione X-MS-Client

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

L'attestazione ADFS rappresenta il protocollo utilizzato dal client finali, che corrisponde a regime di controllo per l'applicazione in uso.  L'attestazione viene popolata con un'intestazione HTTP attualmente impostata solo per Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per ADFS. A seconda dell'applicazione, il valore di questa attestazione sarà uno dei seguenti:



- Per i dispositivi che usano Exchange Active Sync, il valore è Microsoft.Exchange.ActiveSync. 
- L'utilizzo del client di Microsoft Outlook può generare in uno dei valori seguenti:
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

L'attestazione AD FS fornisce una stringa per rappresentare il tipo di dispositivo che utilizza il client di accedere al servizio. Può essere utilizzato quando si desiderano impedire l'accesso per alcuni dispositivi (ad esempio determinati tipi di Smartphone) ai clienti.  L'attestazione viene popolata con un'intestazione HTTP attualmente impostata solo per Exchange Online, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per ADFS. I valori di esempio per l'attestazione includono (ma non sono limitati a) valori riportati di seguito.
>! [Nota] Di seguito sono riportati esempi di cosa potrebbe contenere il valore x-ms-user-agent per un client con cui x-ms-client-application "Microsoft.Exchange.ActiveSync"

- Vortex/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0,3

>! [Nota] È anche possibile che questo valore è vuoto.


## <a name="x-ms-proxy"></a>X-MS-Proxy

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

L'attestazione ADFS indica che la richiesta è passata attraverso il proxy server federativo.  L'attestazione viene popolata con il proxy server federativo, che consente di popolare l'intestazione quando si passa la richiesta di autenticazione per il back-end del servizio federativo. AD FS quindi lo converte in un'attestazione. 

Il valore dell'attestazione è il nome DNS del proxy server federativo che ha trasmesso la richiesta.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-percorso assoluto (vs Active passivo)

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Questo tipo di attestazione può essere utilizzato per determinare le richieste provenienti dai client (RTF) "active" e "passivo" client (browser basata su web). In questo modo richieste esterne dalle applicazioni basate su browser quali Outlook Web Access, SharePoint Online o il portale di Office 365 affinché sia consentita durante le richieste provenienti da rich client, ad esempio Microsoft Outlook sono bloccate.

Il valore dell'attestazione è il nome del servizio ADFS che ha ricevuto la richiesta.
