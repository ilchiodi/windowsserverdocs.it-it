---
title: Panoramica di TLS/SSL (SSP Schannel)
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 51e3886339b7864951b322d210e1a05bef4dc1e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403370"
---
# <a name="tlsssl-overview-schannel-ssp"></a>Panoramica di TLS/SSL (SSP Schannel)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

Questo argomento per i professionisti IT introduce le implementazioni TLS e SSL in Windows tramite il provider di servizi di sicurezza Schannel (SSP), descrivendo le applicazioni pratiche, le modifiche all'implementazione di Microsoft e i requisiti software, oltre a risorse aggiuntive per Windows Server 2012 e Windows 8.

## <a name="BKMK_OVER"></a>Descrizione
Schannel è un Security Support Provider (SSP) che implementa i protocolli di autenticazione standard Internet Secure Sockets Layer (SSL) e Transport Layer Security (TLS).

Security Support Provider Interface (SSPI) è un'API utilizzata dai sistemi Windows per eseguire funzioni correlate alla sicurezza, tra cui l'autenticazione. SSPI funge da interfaccia comune per diversi SSP, incluso SSP Schannel.

Le versioni di TLS 1,0, 1,1 e 1,2, le versioni SSL 2,0 e 3,0, nonché il datagramma Transport Layer Security \(DTLS @ no__t-1 protocollo versione 1,0 e il protocollo Private Communications Transport \(PCT @ no__t-3 si basano sulla crittografia a chiave pubblica. La suite di protocolli di autenticazione Schannel include questi protocolli. Tutti i protocolli Schannel usano un modello client/server.

## <a name="BKMK_APP"></a>Applicazioni
Un problema dell'amministrazione di una rete riguarda la protezione dei dati inviati tra le applicazioni su una rete non attendibile. È possibile usare TLS e SSL per autenticare i server e i computer client e quindi usare il protocollo per crittografare i messaggi tra le entità autenticate.

Ad esempio, è possibile usare TLS/SSL per:

-   Transazioni protette da SSL con un sito Web di e-commerce
-   Accesso client autenticato a un sito Web protetto da SSL
-   Accesso remoto
-   Accesso SQL
-   Posta elettronica

## <a name="BKMK_SOFT"></a>Requisiti
I protocolli TLS e SSL usano un modello client/server e si basano sull'autenticazione del certificato, che richiede un'infrastruttura a chiave pubblica.

## <a name="BKMK_INSTALL"></a>Informazioni Server Manager
Non sono necessari passaggi di configurazione per implementare TLS, SSL o Schannel.

## <a name="see-also"></a>Vedere anche ##

-   [Pacchetto di protezione SChannel](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [Canale sicuro](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [Protocollo Transport Layer Security](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
