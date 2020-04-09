---
title: Protocollo di sicurezza del livello trasporto Datagram
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: d0c066b063cbfc8def54c2e0d02cbb0eaf7f1d40
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852924"
---
# <a name="datagram-transport-layer-security-protocol"></a>Protocollo di sicurezza del livello trasporto Datagram

Windows Server (Canale semestrale), Windows Server 2016, Windows 10

Questo argomento di riferimento per i professionisti IT descrive il protocollo datagramma Transport Layer Security (DTLS), che fa parte del provider SSP (Security Support Provider) Schannel.

## <a name="BKMK_DTLS"></a>
Introdotta in SSP Schannel in Windows Server 2012 e Windows 8, il protocollo DTLS fornisce privacy per la comunicazione per i protocolli di datagramma. Per informazioni su quale versione di DTLS è supportata nelle versioni di Windows, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). Il protocollo consente alle applicazioni client e server di comunicare in un modo progettato per impedire l'intercettazione, la manomissione o la contraffazione dei messaggi. Il protocollo DTLS è basato sul protocollo TLS (Transport Layer Security) e fornisce garanzie di sicurezza equivalenti, riducendo la necessità di usare IPsec o di progettare un protocollo di sicurezza a livello dell'applicazione personalizzata.

I datagrammi sono comuni nei flussi multimediali, come il gioco o la videoconferenza protetta. Gli sviluppatori possono sviluppare applicazioni per l'utilizzo del protocollo DTLS nel contesto del modello SSPI (Security Support Provider Interface) di autenticazione di Windows per proteggere la comunicazione tra client e server. Il protocollo DTLS è basato sul protocollo UDP (User Datagram Protocol). DTLS è progettato per essere il più possibile simile a TLS per ridurre al minimo la nuova invenzione della sicurezza e per massimizzare la quantità di codice e il riutilizzo dell'infrastruttura.

I pacchetti di crittografia disponibili per la configurazione vengono modellati dopo quelli che è possibile configurare per TLS. RC4 non è consentito. Schannel continua a usare Cryptography Next Generation (CNG). Questo sfrutta i vantaggi della certificazione FIPS 140, introdotta in Windows Vista.

## <a name="see-also"></a>Vedere anche

[Transport Layer Security datagramma IETF RFC 4347](http://tools.ietf.org/html/rfc4347)


                                        