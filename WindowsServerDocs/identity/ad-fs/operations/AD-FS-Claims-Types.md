---
title: Tipi di attestazione di accesso client in AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d73995b118ec41ffc892700858d20798f637d83b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857484"
---
# <a name="client-access-policy-claim-types-in-ad-fs"></a>Tipi di attestazione dei criteri di accesso client in AD FS

Per fornire informazioni aggiuntive sul contesto della richiesta, i criteri di accesso client utilizzano i tipi di attestazione seguenti, che AD FS genera dalle informazioni di intestazione della richiesta per l'elaborazione.  Per ulteriori informazioni, vedere [il ruolo del motore delle attestazioni](../technical-reference/the-role-of-the-claims-engine.md).

## <a name="x-ms-forwarded-client-ip"></a>X-MS-Inoltred-client-IP

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`

Questa attestazione AD FS rappresenta un "tentativo migliore" di verificare l'indirizzo IP dell'utente (ad esempio, il client Outlook) che effettua la richiesta. Questa attestazione può contenere più indirizzi IP, incluso l'indirizzo di ogni proxy che ha inviato la richiesta.  Questa attestazione viene popolata da un'intestazione HTTP attualmente impostata solo da Exchange Online, che popola l'intestazione quando passa la richiesta di autenticazione a AD FS. Il valore dell'attestazione può essere uno dei seguenti:


- Un singolo indirizzo IP, ovvero l'indirizzo IP del client connesso direttamente a Exchange Online

    >! Si noti L'indirizzo IP di un client nella rete aziendale verrà visualizzato come indirizzo IP dell'interfaccia esterna del proxy o del gateway dell'organizzazione in uscita.

- Uno o più indirizzi IP
  - Se Exchange Online non è in grado di determinare l'indirizzo IP del client che si connette, il valore verrà impostato in base al valore dell'intestazione x-inoltro-for, un'intestazione non standard che può essere inclusa nelle richieste basate su HTTP ed è supportata da molti client, bilanciamenti del carico e proxy sul mercato.
  - Più indirizzi IP che indicano l'indirizzo IP del client e l'indirizzo di ogni proxy che ha superato la richiesta saranno separati da una virgola.

    >! Si noti Gli indirizzi IP correlati all'infrastruttura di Exchange Online non saranno presenti nell'elenco.


>! Avviso Exchange Online supporta attualmente solo indirizzi IPV4. non supporta gli indirizzi IPV6. 


## <a name="x-ms-client-application"></a>X-MS-client-applicazione

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`

Questa attestazione AD FS rappresenta il protocollo utilizzato dal client finale, che corrisponde liberamente all'applicazione utilizzata.  Questa attestazione viene popolata da un'intestazione HTTP attualmente impostata solo da Exchange Online, che popola l'intestazione quando passa la richiesta di autenticazione a AD FS. A seconda dell'applicazione, il valore di questa attestazione sarà uno dei seguenti:



- Nel caso di dispositivi che usano Exchange Active Sync, il valore è Microsoft. Exchange. ActiveSync. 
- L'utilizzo del client Microsoft Outlook può causare uno dei valori seguenti:
    - Microsoft. Exchange. AutoDiscovery
    - Microsoft. Exchange. OfflineAddressBook
    - Microsoft. Exchange. RPC
    - Microsoft. Exchange. WebServices
    - Microsoft. Exchange. MAPI
- Gli altri valori possibili per questa intestazione includono i seguenti:
    - Microsoft. Exchange. PowerShell
    - Microsoft. Exchange. SMTP
    - Microsoft. Exchange. PopImap
    - Microsoft. Exchange. pop
    - Microsoft. Exchange. IMAP

## <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

Questa attestazione AD FS fornisce una stringa per rappresentare il tipo di dispositivo utilizzato dal client per accedere al servizio. Questo può essere usato quando i clienti vogliono impedire l'accesso per determinati dispositivi, ad esempio tipi specifici di smartphone.  Questa attestazione viene popolata da un'intestazione HTTP attualmente impostata solo da Exchange Online, che popola l'intestazione quando passa la richiesta di autenticazione a AD FS. I valori di esempio per questa attestazione includono (ma non sono limitati) i valori riportati di seguito.
>! Si noti Di seguito sono riportati alcuni esempi di ciò che il valore x-ms-User-Agent potrebbe contenere per un client il cui x-MS-client-Application è "Microsoft. Exchange. ActiveSync".

- Vortice/1.0
- Apple-iPad1C1/812.1
- Apple-iPhone3C1/811.2
- Apple-iPhone/704.11
- Moto-DROID2/4.5.1
- SAMSUNGSPHD700/100.202
- Android/0.3

>! Si noti È anche possibile che questo valore sia vuoto.


## <a name="x-ms-proxy"></a>X-MS-proxy

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

Questa attestazione AD FS indica che la richiesta è passata attraverso il proxy server federativo.  Questa attestazione viene popolata dal proxy server federativo, che popola l'intestazione quando passa la richiesta di autenticazione al back-end Servizio federativo. AD FS quindi la converte in un'attestazione. 

Il valore dell'attestazione è il nome DNS del proxy server federativo che ha superato la richiesta.

## <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-endpoint-Absolute-Path (attivo rispetto a passivo)

Tipo di attestazione: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

Questo tipo di attestazione può essere utilizzato per determinare le richieste originate da client "attivi" (avanzati) rispetto ai client "passivi" (basati su Web browser). In questo modo è possibile consentire le richieste esterne da applicazioni basate su browser, ad esempio Outlook Accesso Web, SharePoint Online o il portale di Office 365, mentre le richieste provenienti da client avanzati come Microsoft Outlook sono bloccate.

Il valore dell'attestazione è il nome del servizio AD FS che ha ricevuto la richiesta.
