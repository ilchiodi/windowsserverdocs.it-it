---
title: Protocollo di sicurezza del livello trasporto Datagram
description: Sicurezza di Windows Server
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
ms.date: 05/16/2018
ms.openlocfilehash: b32ebafff5e41d5c3140f008f6f391852f2f3474
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870792"
---
# <a name="datagram-transport-layer-security-protocol"></a>Protocollo di sicurezza del livello trasporto Datagram

Windows Server (canale semestrale), Windows Server 2016, Windows 10

Questo argomento di riferimento per i professionisti IT descrive il protocollo Datagram Transport Layer Security (DTLS), che fa parte di Schannel Security Support Provider (SSP).

## <a name="BKMK_DTLS"></a>
Introdotta in SSP Schannel in Windows Server 2012 e Windows 8, il protocollo DTLS fornisce la riservatezza di comunicazione per i protocolli di datagramma. Per informazioni su quali DTLS versione è supportata nelle versioni di Windows, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx). Il protocollo consente alle applicazioni client e server di comunicare in un modo progettato per impedire l'intercettazione, la manomissione o la contraffazione dei messaggi. Il protocollo DTLS è basato sul protocollo TLS (Transport Layer Security) e fornisce garanzie di sicurezza equivalenti, riducendo la necessità di usare IPsec o di progettare un protocollo di sicurezza a livello dell'applicazione personalizzata.

I datagrammi sono comuni nei flussi multimediali, ad esempio protetta o i giochi le videoconferenze. Gli sviluppatori possono sviluppare applicazioni per utilizzare il protocollo DTLS all'interno del contesto del modello Windows autenticazione Security Support Provider Interface (SSPI) per proteggere la comunicazione tra client e server. Il protocollo DTLS è basato sul protocollo UDP (User Datagram). DTLS è progettato per essere il più simili a TLS possibile per ridurre al minimo invenzione di protezione nuovo e per ottimizzare la quantità di riutilizzo di codice e dell'infrastruttura.

I pacchetti di crittografia che sono disponibili per la configurazione vengono creati dopo quelli che è possibile configurare per TLS. RC4 non è consentito. Schannel continua a usare Cryptography Next Generation (CNG). Questo sfrutta i vantaggi della certificazione FIPS 140, introdotta in Windows Vista.

## <a name="see-also"></a>Vedere anche

[IETF RFC 4347 Datagram Transport Layer Security](http://tools.ietf.org/html/rfc4347)


