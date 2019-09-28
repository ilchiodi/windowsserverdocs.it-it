---
title: Protocollo di sicurezza del livello trasporto
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: aca2db3ae5bf424dd0f855d24c1ef771039c8b14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403372"
---
# <a name="transport-layer-security-protocol"></a>Protocollo di sicurezza del livello trasporto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

Questo argomento per i professionisti IT descrive il funzionamento del protocollo Transport Layer Security (TLS) e fornisce collegamenti alle RFC IETF per TLS 1,0, TLS 1,1 e TLS 1,2.

I protocolli TLS (e SSL) si trovano tra il livello del protocollo dell'applicazione e il livello TCP/IP, in cui è possibile proteggere e inviare i dati dell'applicazione al livello di trasporto. Poiché i protocolli funzionano tra il livello applicazione e il livello di trasporto, TLS e SSL possono supportare più protocolli a livello di applicazione.

In TLS e SSL si presuppone che sia in uso un trasporto orientato alla connessione, in genere TCP. Il protocollo consente alle applicazioni client e server di rilevare i rischi di sicurezza seguenti:

-   Manomissione di messaggi

-   Intercettazione messaggi

-   Falsificazione del messaggio

I protocolli TLS e SSL possono essere divisi in due livelli. Il primo livello è costituito dal protocollo dell'applicazione e dai tre protocolli di handshake: il protocollo di handshake, il protocollo di crittografia delle modifiche e il protocollo di avviso. Il secondo livello è il protocollo di record. Nell'immagine seguente vengono illustrati i vari livelli e i relativi elementi.

**Livelli del protocollo TLS e SSL**


SSP Schannel implementa i protocolli TLS e SSL senza alcuna modifica. Il protocollo SSL è proprietario, ma Internet Engineering Task Force produce le specifiche TLS pubbliche. Per informazioni su quale versione di TLS o SSL è supportata nelle versioni di Windows, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). La tabella seguente elenca le specifiche per ogni versione di TLS. Ogni specifica contiene informazioni su:

-   Protocollo di record TLS

-   Protocolli di handshake TLS: \- modificare il protocollo di avviso \-

-   Calcoli crittografici

-   Pacchetti di crittografia obbligatori

-   Protocollo dati applicazione

[RFC 5246-la versione 1,2 del protocollo Transport Layer Security (TLS)](http://tools.ietf.org/html/rfc5246)

[RFC 4346-la versione 1,1 del protocollo Transport Layer Security (TLS)](http://tools.ietf.org/html/rfc4346)

[RFC 2246-protocollo TLS versione 1,0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>Ripresa della sessione TLS
Introdotta in Windows Server 2012 R2, SSP Schannel ha implementato la parte lato server della ripresa della sessione TLS. L'implementazione sul lato client di RFC 5077 è stata aggiunta in Windows 8.

I dispositivi che connettono TLS ai server devono riconnettersi spesso. La ripresa della sessione TLS riduce il costo di stabilire le connessioni TLS perché la ripresa comporta un handshake TLS abbreviato. Ciò facilita un maggior numero di tentativi di ripresa consentendo a un gruppo di server TLS di riprendere le sessioni TLS di ogni altro. Questa modifica offre i risparmi seguenti per qualsiasi client TLS che supporta RFC 5077, inclusi Windows Phone e i dispositivi Windows RT:

-   Riduzione dell'utilizzo di risorse nel server.

-   Riduzione della larghezza di banda, che determina un miglioramento dell'efficienza delle connessioni client.

-   Riduzione del tempo impiegato per l'handshake TLS a causa delle riprese della connessione

Per informazioni sulla ripresa della sessione TLS senza stato, vedere il documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>Negoziazione del protocollo applicativo
 In Windows Server 2012 R2 e Windows 8.1 è stato introdotto il supporto che consente la negoziazione del protocollo applicativo TLS sul lato client. Le applicazioni possono sfruttare i protocolli come parte dello sviluppo standard HTTP 2,0 e gli utenti possono accedere a Servizi online come Google e Twitter usando le app che eseguono il protocollo SPDY.

Per informazioni sul funzionamento della negoziazione del protocollo applicativo, vedere l'[estensione di negoziazione del protocollo a livello di applicazione TLS (Transport Layer Security)](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="BKMK_SNI"></a>Supporto TLS per le estensioni Indicazione nome server
La funzionalità Indicazione nome server (SNI, Server Name Indication) estende i protocolli SSL e TLS per consentire la corretta identificazione del server quando su un singolo server sono in esecuzione numerose immagini virtuali. In uno scenario di hosting virtuale, diversi domini, ognuno con il proprio certificato potenzialmente distinto, sono ospitati in un unico server. In questo caso, il server non ha modo di sapere in anticipo quale certificato inviare al client. SNI consente al client di informare il dominio di destinazione in precedenza nel protocollo e questo consente al server di selezionare correttamente il certificato appropriato.

Questa funzionalità aggiuntiva:

-   Consente di ospitare più siti Web SSL in una singola combinazione di protocollo e porta Internet

-   Riduce l'utilizzo di memoria quando più siti Web SSL sono ospitati in un singolo server Web.

-   Consente a più utenti di connettersi ai siti Web SSL contemporaneamente



