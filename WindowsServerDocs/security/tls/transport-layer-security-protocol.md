---
title: Protocollo di sicurezza del livello trasporto
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: 0a3b241fe0d2a61361d551b7f515507ad55d71cd
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284236"
---
# <a name="transport-layer-security-protocol"></a>Protocollo di sicurezza del livello trasporto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

Questo argomento destinato ai professionisti IT descrive come il protocollo Transport Layer Security (TLS) funziona e vengono forniti i collegamenti per le specifiche IETF RFC per TLS 1.0, TLS 1.1 e TLS 1.2.

I protocolli TLS (e SSL) si trovano tra il livello di protocollo dell'applicazione e il livello di TCP/IP, in cui possono proteggere e inviare i dati dell'applicazione a livello di trasporto. Poiché i protocolli di operare tra il livello dell'applicazione e il livello di trasporto, TLS e SSL può supportare più protocolli a livello dell'applicazione.

TLS e SSL si supponga che un trasporto orientato alla connessione, in genere TCP, è in uso. Il protocollo consente alle applicazioni client e server rilevare i rischi di sicurezza seguenti:

-   Manomissione del messaggio

-   Intercettazione dei messaggi

-   Contraffazione dei messaggi

I protocolli TLS e SSL possono essere suddivisi in due livelli. Il primo livello è costituito da tre protocolli la sincronizzazione e il protocollo dell'applicazione: il protocollo di handshake, il protocollo specifiche pacchetti di crittografia di modifica e il protocollo di avviso. Il secondo livello è il protocollo di record. L'immagine seguente illustra i diversi livelli e i relativi elementi.

**Livelli del protocollo TLS e SSL**


SSP Schannel implementa i protocolli TLS e SSL senza alcuna modifica. Il protocollo SSL è proprietario, ma Internet Engineering Task Force produce le specifiche di TLS pubbliche. Per informazioni su quali TLS o SSL versione è supportata nelle versioni di Windows, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). La tabella seguente elenca le specifiche per ogni versione TLS. Ogni specifica contiene informazioni su:

-   Il protocollo di Record TLS

-   I protocolli di handshake TLS: \- Modificare cipher spec protocollo \- protocollo di avviso

-   Calcoli di crittografia

-   Pacchetti di crittografia obbligatorie

-   Protocollo di dati dell'applicazione

[RFC 5246 - il protocollo Transport Layer Security (TLS) versione 1.2](http://tools.ietf.org/html/rfc5246)

[RFC 4346 - il protocollo Transport Layer Security (TLS) versione 1.1](http://tools.ietf.org/html/rfc4346)

[RFC 2246 - protocollo TLS versione 1.0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>Ripresa della sessione TLS
Introdotta in Windows Server 2012 R2, SSP Schannel implementata la parte di server-side di ripresa della sessione TLS. L'implementazione lato client di RFC 5077 è stata aggiunta in Windows 8.

I dispositivi che connettono TLS ai server devono riconnettersi spesso. Ripresa della sessione TLS riduce il costo di ristabilire le connessioni TLS perché prevede un handshake TLS abbreviato. Ciò semplifica i tentativi di ripresa, consentendo a un gruppo di server di riprendere le rispettive sessioni TLS. Questa modifica offre i seguenti risparmi per qualsiasi client TLS che supporta RFC 5077, inclusi i dispositivi Windows Phone e Windows RT:

-   Riduzione dell'utilizzo di risorse nel server.

-   Riduzione della larghezza di banda, che determina un miglioramento dell'efficienza delle connessioni client.

-   Riduzione del tempo impiegato per l'handshake TLS a causa delle riprese della connessione

Per informazioni sulla ripresa della sessione TLS senza stato, vedere il documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>Negoziazione del protocollo applicativo
 Windows 8.1 e Windows Server 2012 R2 introdotto il supporto che consenta di negoziazione del protocollo applicativo TLS sul lato client. Le applicazioni possono sfruttare i protocolli come parte dello sviluppo standard HTTP 2.0 e gli utenti possono accedere i servizi online come Google e Twitter usando le App in esecuzione il protocollo SPDY.

Per informazioni sul funzionamento della negoziazione del protocollo applicativo, vedere l'[estensione di negoziazione del protocollo a livello di applicazione TLS (Transport Layer Security)](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="BKMK_SNI"></a>Supporto TLS per estensioni di indicazione nome Server
La funzionalità Indicazione nome server (SNI, Server Name Indication) estende i protocolli SSL e TLS per consentire la corretta identificazione del server quando su un singolo server sono in esecuzione numerose immagini virtuali. In uno scenario di hosting virtuale diversi domini, ognuno con il proprio certificato potenzialmente distinto, sono ospitati in un unico server. In questo caso, il server non ha modo di sapere in anticipo quale certificato per inviare al client. SNI consente al client di informare il dominio di destinazione in precedenza nella specifica del protocollo e questo consente al server di selezionare correttamente il certificato appropriato.

Questa funzionalità aggiuntiva:

-   Puoi ospitare più siti Web SSL in una combinazione di porta e protocollo Internet single

-   Riduce l'utilizzo di memoria quando più siti Web SSL sono ospitati in un singolo server Web.

-   Consente a più utenti di connettersi a siti Web SSL contemporaneamente



