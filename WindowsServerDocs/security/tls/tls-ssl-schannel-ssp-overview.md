---
title: Panoramica TLS/SSL (SSP Schannel)
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: a6571e5e06e07fd62ad4cf39bab322b45c90a9f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848602"
---
# <a name="tlsssl-overview-schannel-ssp"></a>Panoramica TLS/SSL (SSP Schannel)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

In questo argomento per i professionisti IT illustra le implementazioni SSL e TLS in Windows tramite Security Service Provider (SSP Schannel) applicazioni pratiche, le modifiche nell'implementazione di Microsoft e i requisiti software, oltre a risorse aggiuntive per Windows Server 2012 e Windows 8.

## <a name="BKMK_OVER"></a>Descrizione
Schannel è un Security Support Provider (SSP) che implementa i protocolli di autenticazione standard Internet Secure Sockets Layer (SSL) e Transport Layer Security (TLS).

Security Support Provider Interface (SSPI) è un'API utilizzata dai sistemi Windows per eseguire funzioni correlate alla sicurezza, tra cui l'autenticazione. SSPI funziona come un'interfaccia comune per diversi provider di servizi condivisi, tra cui SSP. Schannel.

Le versioni 1.0, 1.1 e 1.2, le versioni SSL 2.0 e 3.0, TLS, nonché il protocollo Datagram Transport Layer Security \(DTLS\) protocollo versione 1.0 e il trasporto delle comunicazioni privato \(PCT\) protocollo si basano su crittografia a chiave pubblica. La suite di protocolli di autenticazione Schannel include questi protocolli. Tutti i protocolli Schannel usano un modello client/server.

## <a name="BKMK_APP"></a>Applicazioni
Un problema dell'amministrazione di una rete riguarda la protezione dei dati inviati tra le applicazioni su una rete non attendibile. È possibile usare TLS e SSL per autenticare server e computer client e quindi usare il protocollo per crittografare i messaggi tra le entità autenticate.

Ad esempio, è possibile usare TLS/SSL per:

-   Transazioni protette da SSL con un sito Web di e-commerce
-   Accesso client autenticato a un sito Web protetto da SSL
-   Accesso remoto
-   Accesso SQL
-   Posta elettronica

## <a name="BKMK_SOFT"></a>Requisiti
I protocolli TLS e SSL usano un modello client/server e si basano sull'autenticazione del certificato, che richiede un'infrastruttura a chiave pubblica.

## <a name="BKMK_INSTALL"></a>Informazioni su Server Manager
Non sono necessarie per implementare TLS, SSL o Schannel alcun passaggio di configurazione.

## <a name="see-also"></a>Vedere anche ##

-   [Pacchetto di protezione Schannel](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [Canale sicuro](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [Protocollo di sicurezza Layer di trasporto](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
