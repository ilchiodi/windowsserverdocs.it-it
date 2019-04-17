---
title: Protocollo Datagram Transport Layer Security
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 31c1cf1f3218c0511a4407b560be30d0c6f86233
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# Protocollo Datagram Transport Layer Security

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento di riferimento per i professionisti IT descrive il protocollo Datagram Transport Layer Security (DTLS), che fa parte di Schannel Security Support Provider (SSP).

## <a name="BKMK_DTLS"></a>
Introdotto in SSP Schannel in Windows Server 2012 e Windows 8, il protocollo DTLS fornisce la riservatezza delle comunicazioni per i protocolli di datagramma. Per informazioni su quali DTLS versione è supportata nelle versioni di Windows, vedere [i protocolli TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx). Il protocollo consente alle applicazioni client e server di comunicare in modo che è progettato per impedire l'intercettazione, la manomissione o la contraffazione dei messaggi. Il protocollo DTLS è basato sul protocollo Transport Layer Security (TLS) e fornisce garanzie di sicurezza equivalenti, riducendo la necessità di usare IPsec o di progettare un protocollo di sicurezza di livello dell'applicazione personalizzata.

I datagrammi sono comuni nei flussi multimediali, ad esempio le videoconferenze protette o i giochi. Gli sviluppatori possono sviluppare applicazioni di utilizzare il protocollo DTLS nel contesto del modello di Security Support Provider Interface (SSPI) di autenticazione di Windows per proteggere la comunicazione tra client e server. Il protocollo DTLS è basato sul protocollo UDP (User Datagram). DTLS è progettato per essere possibile per ridurre al minimo invenzione protezione nuovo e ottimizzare la quantità di riutilizzo del codice e dell'infrastruttura come simile a TLS.

I pacchetti di crittografia che sono disponibili per la configurazione vengono creati dopo quelli che è possibile configurare per TLS. RC4 non è consentito. Schannel continua a usare Cryptography Next Generation (CNG). Questo si avvale della certificazione FIPS 140, introdotta in Windows Vista.

## Vedere anche

[IETF RFC 4347 Datagram Transport Layer Security](http://tools.ietf.org/html/rfc4347)


