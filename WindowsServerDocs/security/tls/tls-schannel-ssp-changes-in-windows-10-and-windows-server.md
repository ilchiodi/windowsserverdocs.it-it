---
title: TLS (SSP Schannel)
ms.prod: windows-server
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 05/16/2018
ms.openlocfilehash: 3547c77e8c58bcbb219a7b017c3186f198007805
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820164"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Modifiche TLS (SSP Schannel) in Windows 10 e Windows Server 2016

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016 e Windows 10

## <a name="cipher-suite-changes"></a>Modifiche del pacchetto di crittografia

Windows 10, versione 1511 e Windows Server 2016 aggiungono il supporto per la configurazione dell'ordine dei pacchetti di crittografia tramite la gestione di dispositivi mobili (MDM).

Per le modifiche all'ordine di priorità del pacchetto di crittografia, vedere pacchetti [di crittografia in Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).

Aggiunta del supporto per i pacchetti di crittografia seguenti:

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) in Windows 10, versione 1507 e Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) in Windows 10, versione 1507 e Windows Server 2016

DisabledByDefault modifiche per i pacchetti di crittografia seguenti:

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) in Windows 10, versione 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) in Windows 10, versione 1703
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) in Windows 10, versione 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) in Windows 10, versione 1703
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) in Windows 10, versione 1703
- TLS_RSA_WITH_RC4_128_SHA in Windows 10 versione 1709
- TLS_RSA_WITH_RC4_128_MD5 in Windows 10 versione 1709

A partire da Windows 10, versione 1507 e Windows Server 2016, i certificati SHA 512 sono supportati per impostazione predefinita.

### <a name="rsa-key-changes"></a>Modifiche della chiave RSA

Windows 10, versione 1507 e Windows Server 2016 aggiungono le opzioni di configurazione del registro di sistema per le dimensioni delle chiavi RSA del client.

Per altre informazioni, vedere [KeyExchangeAlgorithm-dimensioni delle chiavi RSA del client](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Modifiche della chiave Diffie-Hellman

Windows 10, versione 1507 e Windows Server 2016 aggiungono le opzioni di configurazione del registro di sistema per le dimensioni della chiave Diffie-Hellman.

Per ulteriori informazioni, vedere la pagina relativa alle [dimensioni delle chiavi KeyExchangeAlgorithm-Diffie-Hellman](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="sch_use_strong_crypto-option-changes"></a>Modifica opzioni di SCH_USE_STRONG_CRYPTO

Con Windows 10, versione 1507 e Windows Server 2016, [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx) opzione Disabilita ora le crittografie null, MD5, des ed export.

## <a name="elliptical-curve-changes"></a>Modifiche a curva ellittica

Windows 10, versione 1507 e Windows Server 2016 aggiungono Criteri di gruppo configurazione per le curve ellittiche in configurazione computer > Modelli amministrativi > rete > le impostazioni di configurazione SSL. L'elenco di ordine di curva ECC specifica l'ordine in cui le curve ellittiche sono preferite e Abilita le curve supportate che non sono abilitate. 
 
Aggiunta del supporto per le curve ellittiche seguenti:

- BrainpoolP256r1 (RFC 7027) in Windows 10, versione 1507 e Windows Server 2016
- BrainpoolP384r1 (RFC 7027) in Windows 10, versione 1507 e Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) in Windows 10, versione 1507 e Windows Server 2016
- Curve25519 (RFC draft-ietf-TLS-Curve25519) in Windows 10, versione 1607 e Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Supporto del livello di invio per SealMessage & UnsealMessage

Windows 10, versione 1507 e Windows Server 2016 aggiungono il supporto per SealMessage/UnsealMessage a livello di distribuzione.

## <a name="dtls-12"></a>DTLS 1,2

Windows 10, versione 1607 e Windows Server 2016 aggiungono il supporto per DTLS 1,2 (RFC 6347).

## <a name="httpsys-thread-pool"></a>HTTP. Pool di thread SYS 

Windows 10, versione 1607 e Windows Server 2016 aggiungono la configurazione del registro di sistema delle dimensioni del pool di thread usato per gestire gli handshake TLS per HTTP. SYS.

Percorso del Registro di sistema: 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

Per specificare le dimensioni massime del pool di thread per core CPU, creare una voce **MaxAsyncWorkerThreadsPerCpu** . Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD con le dimensioni desiderate. Se non è configurato, il valore massimo è 2 thread per core CPU.

## <a name="next-protocol-negotiation-npn-support"></a>Supporto per la negoziazione del protocollo successivo (NPN)

A partire da Windows 10 versione 1703, la negoziazione del protocollo successiva (NPN) è stata rimossa e non è più supportata.

## <a name="pre-shared-key-psk"></a>Chiave pre-condivisa (PSK)

Windows 10, versione 1607 e Windows Server 2016 aggiungono il supporto per l'algoritmo di scambio delle chiavi PSK (RFC 4279).

Aggiunta del supporto per i pacchetti di crittografia PSK seguenti:

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_CBC_SHA384 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Ripresa della sessione senza miglioramenti delle prestazioni sul lato server dello stato lato server

Windows 10, versione 1507 e Windows Server 2016 forniscono il 30% più riprese di sessione al secondo con ticket di sessione rispetto a Windows Server 2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hash della sessione ed estensione master secret estesa

Windows 10, versione 1507 e Windows Server 2016 aggiungono il supporto per RFC 7627: hash della sessione Transport Layer Security (TLS) ed estensione master secret estesa.

A causa di questa modifica, Windows 10 e Windows Server 2016 richiedono aggiornamenti del [provider SSL CNG](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx) di terze parti per supportare NCRYPT_SSL_INTERFACE_VERSION_3 e per descrivere questa nuova interfaccia.


## <a name="ssl-support"></a>Supporto di SSL

A partire da Windows 10, versione 1607 e Windows Server 2016, il client TLS e il server SSL 3,0 sono disabilitati per impostazione predefinita. Ciò significa che, a meno che l'applicazione o il servizio non richieda specificamente SSL 3,0 tramite SSPI, il client non offrirà né accetterà mai SSL 3,0 e il server non selezionerà mai SSL 3,0.

A partire da Windows 10 versione 1607 e Windows Server 2016, SSL 2,0 è stato rimosso e non è più supportato.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Modifiche alla conformità TLS di Windows ai requisiti di TLS 1,2 per le connessioni con client TLS non conformi

In TLS 1,2 il client usa l' [estensione "signature_algorithms"](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) per indicare al server quali coppie di algoritmi di firma/hash possono essere usate nelle firme digitali, ad esempio i certificati server e lo scambio di chiave del server. Per la RFC di TLS 1,2 è inoltre necessario che l'estensione "signature_algorithms" del messaggio del certificato del server venga applicata:

"Se il client ha fornito un'estensione" signature_algorithms ", tutti i certificati forniti dal server devono essere firmati da una coppia di algoritmi hash/firma visualizzata in tale estensione".

In pratica, alcuni client TLS di terze parti non sono conformi alla RFC TLS 1,2 e non includono tutte le coppie di firma e algoritmo hash che sono disposti ad accettare nell'estensione "signature_algorithms" oppure ometteno completamente l'estensione (quest'ultimo indica al server che il client supporta solo SHA1 con RSA, DSA o ECDSA).

Un server TLS ha spesso un solo certificato configurato per ogni endpoint, il che significa che il server non può sempre fornire un certificato che soddisfi i requisiti del client.

Prima di Windows 10 e Windows Server 2016, lo stack di Windows TLS rispettava rigorosamente i requisiti RFC di TLS 1,2, causando errori di connessione con client TLS non conformi a RFC e problemi di interoperabilità. In Windows 10 e Windows Server 2016 i vincoli sono rilassati e il server può inviare un certificato che non è conforme a TLS 1,2 RFC, se questa è l'unica opzione del server. Il client può quindi continuare o terminare l'handshake.

Quando si convalidano i certificati client e server, lo stack TLS di Windows è strettamente conforme alla RFC TLS 1,2 e consente solo la firma negoziata e gli algoritmi hash nei certificati server e client.


