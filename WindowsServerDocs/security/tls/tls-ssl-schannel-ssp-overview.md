---
title: Panoramica di TLS/SSL (SSP Schannel)
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: e0045edf1d04feabac2354d47132b86e6d27ad2d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853624"
---
# <a name="tlsssl-overview-schannel-ssp"></a>Panoramica di TLS/SSL (SSP Schannel)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows 10

Questo argomento per i professionisti IT introduce le implementazioni TLS e SSL in Windows tramite il provider di servizi di sicurezza Schannel (SSP), descrivendo le applicazioni pratiche, le modifiche all'implementazione di Microsoft e i requisiti software, oltre a risorse aggiuntive per Windows Server 2012 e Windows 8.

## <a name="description"></a><a name="BKMK_OVER"></a>Descrizione
Schannel è un Security Support Provider (SSP) che implementa i protocolli di autenticazione standard Internet Secure Sockets Layer (SSL) e Transport Layer Security (TLS).

Security Support Provider Interface (SSPI) è un'API utilizzata dai sistemi Windows per eseguire funzioni correlate alla sicurezza, tra cui l'autenticazione. SSPI funge da interfaccia comune per diversi SSP, incluso SSP Schannel.

Le versioni di TLS 1,0, 1,1 e 1,2, le versioni SSL 2,0 e 3,0, nonché il datagramma Transport Layer Security \(DTLS\) protocollo versione 1,0 e il protocollo di trasporto delle comunicazioni private \(PCT\) sono basati sulla crittografia a chiave pubblica. La suite di protocolli di autenticazione Schannel include questi protocolli. Tutti i protocolli Schannel usano un modello client/server.

## <a name="applications"></a><a name="BKMK_APP"></a>Applicazioni
Un problema dell'amministrazione di una rete riguarda la protezione dei dati inviati tra le applicazioni su una rete non attendibile. È possibile usare TLS e SSL per autenticare i server e i computer client e quindi usare il protocollo per crittografare i messaggi tra le entità autenticate.

Ad esempio, è possibile usare TLS/SSL per:

-   Transazioni protette da SSL con un sito Web di e-commerce
-   Accesso client autenticato a un sito Web protetto da SSL
-   Accesso remoto
-   Accesso SQL
-   E-mail

## <a name="requirements"></a><a name="BKMK_SOFT"></a>Requisiti
I protocolli TLS e SSL usano un modello client/server e si basano sull'autenticazione del certificato, che richiede un'infrastruttura a chiave pubblica.

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>Informazioni Server Manager
Non sono necessari passaggi di configurazione per implementare TLS, SSL o Schannel.

## <a name="see-also"></a>Vedere anche ##

-   [Pacchetto di protezione SChannel](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [Canale sicuro](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [Protocollo Transport Layer Security](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
