---
title: Risoluzione dei problemi - Fiddler - WS-Federation di AD FS
description: Questo documento Mostra un'analisi approfondita di uno scambio WS-Federation con AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be1c9f466ec13272d10f0fb9ca31cf326a1ec29a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846902"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>Risoluzione dei problemi - Fiddler - WS-Federation di AD FS
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>Passaggio 1 e 2
Questo è l'inizio del nostro traccia.  In questo frame viene visualizzato quanto segue: ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

Richiesta:

- Richiesta HTTP GET per la relying party (di terze parti http://sql1.contoso.com/SampApp)

Risposta:

- La risposta è HTTP 302 (reindirizzamento).  I dati di trasporto nell'intestazione della risposta mostrano la posizione in cui si viene reindirizzati (https://sts.contoso.com/adfs/ls)
- L'URL di reindirizzamento contiene wa = wsignin 1.0 che indica che l'applicazione relying Party ha compilato una richiesta di accesso WS-Federation per noi e ciò inviati a ADFS/oauth2//ls/endpoint del AD FS.  Questo è noto come reindirizzare l'associazione.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>Passaggio 3 e 4

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

Richiesta:

- HTTP GET al nostro server(sts.contoso.com) AD FS

Risposta:

- La risposta è una richiesta di credenziali.  Ciò indica che si sta usando un sistema di autenticazione form
- Facendo clic su visualizzazione Web della risposta è possibile visualizzare prompt le credenziali.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>Passaggio 5 e 6

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

Richiesta:

- Richiesta HTTP POST con il nome utente e password.  
- Ti presentiamo le credenziali.  Esaminando i dati non elaborati nella richiesta è possibile osservare le credenziali

Risposta:

- La risposta è stato trovato e MSIAuth cookie crittografato viene creata e restituita.  Ciò consente di convalidare l'asserzione SAML prodotta dal cliente.  Questo è noto anche come "cookie di autenticazione" e verrà essere presente solo se ADFS è il provider di identità.


## <a name="step-7-and-8"></a>Passaggio 7 e 8
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

Richiesta:

- Ora che è stato autenticato è eseguire un'altra richiesta HTTP GET per il server AD FS e presentare il token di autenticazione

Risposta:

- La risposta è un HTTP di OK che significa che ADFS ha autenticato l'utente in base alle credenziali fornite
- Inoltre, impostiamo 3 cookie al client
    - MSISAuthenticated contiene un valore di timestamp con codifica base64 per quando il client è stato autenticato.
    - MSISLoopDetectionCookie viene utilizzato il meccanismo di rilevamento di ciclo infinito di AD FS per i client di arresto che hanno finisce un ciclo di reindirizzamento infinito per il Server federativo. I dati del cookie sono un timestamp corrispondente con codificata base64.
    - MSISSignout viene usato per tenere traccia dei provider di identità e RPs tutti visitato per la sessione SSO. Questo cookie viene utilizzato quando viene richiamata una disconnessione WS-Federation. È possibile visualizzare il contenuto di questo cookie utilizzando un decodificatore base64.
    
## <a name="step-9-and-10"></a>Passaggio 9 e 10
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler7.png) Richiesta:

- HTTP POST

Risposta:

- La risposta è un è stato trovato

## <a name="step-11-and-12"></a>Passaggio 11 e 12
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler8.png) Richiesta:

- HTTP GET

Risposta:

- La risposta è OK

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi di AD FS](ad-fs-tshoot-overview.md)