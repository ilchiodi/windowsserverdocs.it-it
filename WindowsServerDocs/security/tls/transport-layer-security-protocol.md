---
title: Protocollo di sicurezza Layer di trasporto
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 94c81a79e5f3fd8fc22eafde3de8bd3d0bdd73f0
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# Protocollo di sicurezza Layer di trasporto

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento destinato ai professionisti IT descrive come funziona e i collegamenti per IETF RFC per TLS 1.0, TLS 1.1 e TLS 1.2, il protocollo Transport Layer Security (TLS).

I protocolli TLS (e SSL) si trovano tra il livello di protocollo dell'applicazione e il livello di TCP/IP, in cui possono proteggere e inviare i dati delle applicazioni per il livello di trasporto. Poiché i protocolli funzionano tra il livello di applicazione e il livello di trasporto, SSL e TLS può supportare più protocolli a livello di applicazione.

TLS e SSL si presuppone un trasporto orientata alla connessione, in genere TCP, in uso. Il protocollo consente alle applicazioni client e server rilevare i rischi di sicurezza seguenti:

-   Intromissioni nei messaggi

-   Intercettazione di messaggi

-   Contraffazione dei messaggi

I protocolli TLS e SSL possono essere suddivisa in due livelli. Il primo livello è costituito da del protocollo dell'applicazione e i protocolli di handshake tre: il protocollo di handshake, il protocollo di specifica di crittografia di modifica e il protocollo di avviso. Il secondo livello è il protocollo di record. L'immagine seguente illustra i diversi livelli e i relativi elementi.

**Livelli di protocollo TLS e SSL**


SSP Schannel implementa i protocolli TLS e SSL senza modifiche. Il protocollo SSL è proprietario, ma Internet Engineering Task Force produce le specifiche di TLS pubbliche. Per informazioni su quali TLS o SSL versione è supportata nelle versioni di Windows, vedere [i protocolli TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx). La tabella seguente elenca le specifiche per ogni versione TLS. Ogni specifica contiene informazioni su:

-   Il protocollo di Record TLS

-   I protocolli di handshake TLS: \-protocollo specifica crittografia modifica \-avviso di protocollo

-   Calcoli crittografici

-   Pacchetti di crittografia obbligatorie

-   Protocollo di dati dell'applicazione

[RFC 5246 - il protocollo Transport Layer Security (TLS) versione 1.2](http://tools.ietf.org/html/rfc5246)

[RFC 4346 - il protocollo Transport Layer Security (TLS) versione 1.1](http://tools.ietf.org/html/rfc4346)

[RFC 2246 - protocollo TLS versione 1.0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>Ripresa della sessione TLS
Introdotto in Windows Server 2012 R2, SSP Schannel implementato la parte del server di ripresa della sessione TLS. L'implementazione sul lato client di RFC 5077 è stata aggiunta in Windows 8.

I dispositivi che connettono spesso TLS ai server devono riconnettersi. Ripresa della sessione TLS riduce il costo di ristabilire le connessioni TLS perché prevede un handshake TLS abbreviato. Questo facilita più tentativi di riavvio della consentendo a un gruppo di server TLS di riprendere le sessioni di altro TLS. Per i client TLS che supporta RFC 5077, inclusi i dispositivi Windows Phone e Windows RT, questa modifica fornisce il risparmio di seguenti:

-   Utilizzo delle risorse ridotto nel server di

-   Riduzione della larghezza di banda, migliorando l'efficienza delle connessioni client

-   Riduzione del tempo impiegato per l'handshake TLS a causa delle riprese della connessione

Per informazioni sulla ripresa di sessione TLS senza stato, vedere il documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>Negoziazione del protocollo applicativo
 Windows Server 2012 R2 e Windows 8.1 è stato introdotto il supporto che consente di negoziazione del protocollo applicativo TLS sul lato client. Le applicazioni possono sfruttare i protocolli come parte dello sviluppo standard HTTP 2.0 e gli utenti possono accedere a servizi online come Google e Twitter usando App che eseguono il protocollo SPDY.

Per informazioni sul funzionamento della negoziazione del protocollo applicativo, vedere [estensione negoziazione del protocollo Transport Layer Security (TLS) dell'applicazione livello](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="BKMK_SNI"></a>Supporto TLS per estensioni di indicazione nome Server
La funzionalità di indicazione nome Server (SNI) estende i protocolli SSL e TLS per consentire la corretta identificazione del server quando numerose immagini virtuali sono in esecuzione in un singolo server. In uno scenario di hosting virtuale diversi domini (ognuno con il proprio certificato potenzialmente distinto) sono ospitati in un server. In questo caso, il server non ha modo di sapere in anticipo quale certificato per inviare al client. SNI consente al client di informare il dominio di destinazione in precedenza nel protocollo e questo consente al server di selezionare correttamente il certificato appropriato.

Questa funzionalità aggiuntiva:

-   Consente di ospitare più siti Web SSL su una combinazione di porta e protocollo Internet singolo

-   Riduce l'utilizzo della memoria quando più siti Web SSL sono ospitati in un singolo server web.

-   Consente a più utenti di connettersi ai siti Web SSL contemporaneamente



