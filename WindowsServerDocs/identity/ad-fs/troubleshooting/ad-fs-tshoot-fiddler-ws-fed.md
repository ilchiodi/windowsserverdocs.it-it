---
title: Risoluzione dei problemi di AD FS-Fiddler-WS-Federation
description: In questo documento viene illustrata una traccia dettagliata di uno scambio WS-Federation con AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d263f48aadff7c77cba44a2328d472ebbe5dfbbf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407219"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>Risoluzione dei problemi di AD FS-Fiddler-WS-Federation
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>Passaggio 1 e 2
Questo è l'inizio della traccia.  In questo frame viene visualizzato quanto segue: ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

Richiesta

- HTTP GET al relying party (http://sql1.contoso.com/SampApp)

Risposta

- La risposta è un HTTP 302 (Reindirizzamento).  I dati di trasporto nell'intestazione della risposta indicano dove reindirizzare (https://sts.contoso.com/adfs/ls)
- L'URL di reindirizzamento contiene WA = wsignin 1,0 che indica che l'applicazione relying party ha compilato una richiesta di accesso WS-Federation per Microsoft e l'ha inviata all'endpoint/ADFS/ls/del AD FS.  Questa operazione è nota come binding di reindirizzamento.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>Passaggio 3 e 4

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

Richiesta

- HTTP GET al server AD FS (STS. contoso. com)

Risposta

- La risposta è una richiesta di credenziali.  Ciò indica che si stanno usando moduli authnetication
- Facendo clic su WebView della risposta, è possibile visualizzare la richiesta di credenziali.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>Passaggio 5 e 6

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

Richiesta

- HTTP POST con il nome utente e la password.  
- Microsoft presenta le proprie credenziali.  Esaminando i dati non elaborati nella richiesta, è possibile visualizzare le credenziali

Risposta

- La risposta viene trovata e il cookie crittografato MSIAuth viene creato e restituito.  Viene usato per convalidare l'asserzione SAML prodotta dal client.  Questo è noto anche come "cookie di autenticazione" e sarà presente solo quando AD FS è il provider di identità.


## <a name="step-7-and-8"></a>Passaggio 7 e 8
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

Richiesta

- Ora che è stata eseguita l'autenticazione, viene eseguita un'altra operazione HTTP GET al server AD FS e viene presentato il token di autenticazione

Risposta

- La risposta è un HTTP OK che indica che AD FS ha autenticato l'utente in base alle credenziali fornite
- Inoltre, vengono impostati 3 cookie sul client
    - MSISAuthenticated contiene un valore di timestamp con codifica Base64 per il momento in cui il client è stato autenticato.
    - MSISLoopDetectionCookie viene usato dal meccanismo di rilevamento del ciclo infinito AD FS per arrestare i client che sono finiti in un ciclo di reindirizzamento infinito al server federativo. I dati del cookie sono un timestamp con codifica Base64.
    - MSISSignout viene usato per tenere traccia del provider di identità e di tutti i relativi visitatori per la sessione SSO. Questo cookie viene utilizzato quando viene richiamata una disconnessione WS-Federation. È possibile visualizzare il contenuto di questo cookie usando un decodificatore Base64.
    
## <a name="step-9-and-10"></a>Passaggio 9 e 10
Richiesta ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler7.png):

- POST HTTP

Risposta

- La risposta è un oggetto trovato

## <a name="step-11-and-12"></a>Passaggio 11 e 12
Richiesta ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler8.png):

- HTTP GET

Risposta

- La risposta è accettabile

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)