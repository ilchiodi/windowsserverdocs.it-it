---
title: TLS (Schannel SSP)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 05/16/2018
ms.openlocfilehash: 030fd81e0c6ba0423f1fa73e680006766cf2b180
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890772"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Modifiche TLS (SSP Schannel) in Windows 10 e Windows Server 2016

>Si applica a: Windows Server (canale semestrale), Windows Server 2016 e Windows 10

## <a name="cipher-suite-changes"></a>Modifiche apportate alla Suite di crittografia

Windows 10, versione 1511 e Windows Server 2016 è possibile aggiungere il supporto per la configurazione dell'ordine dei pacchetti di crittografia con Gestione dispositivi mobili (MDM).

Per le modifiche nell'ordine di priorità suite pacchetti di crittografia, vedere [pacchetti di crittografia in Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).

Aggiunta del supporto per pacchetti di crittografia seguenti:

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) in Windows 10, versione 1507 e Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) in Windows 10, versione 1507 e Windows Server 2016

DisabledByDefault modificare per i pacchetti di crittografia seguenti:

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) in Windows 10 versione 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) in Windows 10 versione 1703
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) in Windows 10 versione 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) in Windows 10 versione 1703
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) in Windows 10 versione 1703
- TLS_RSA_WITH_RC4_128_SHA in Windows 10, versione 1709
- TLS_RSA_WITH_RC4_128_MD5 in Windows 10, versione 1709

A partire da Windows 10, versione 1507 e Windows Server 2016, sono supportati i certificati SHA 512 per impostazione predefinita.

### <a name="rsa-key-changes"></a>Modifiche delle chiavi RSA

Windows 10, versione 1507 e Windows Server 2016 è possibile aggiungere opzioni di configurazione del Registro di sistema per le dimensioni della chiave RSA client.

Per altre informazioni, vedere [KeyExchangeAlgorithm - dimensioni della chiave RSA Client](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Modifiche delle chiavi Diffie-Hellman

Windows 10, versione 1507 e Windows Server 2016 è possibile aggiungere opzioni di configurazione del Registro di sistema per le dimensioni della chiave Diffie-Hellman.

Per altre informazioni, vedere [KeyExchangeAlgorithm - dimensioni della chiave Diffie-Hellman](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="schusestrongcrypto-option-changes"></a>Modifiche apportate alle opzioni SCH_USE_STRONG_CRYPTO

Con Windows 10, versione 1507 e Windows Server 2016 [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx) opzione ora Disabilita NULL, MD5, DES ed esportare le crittografie.

## <a name="elliptical-curve-changes"></a>Modifiche di curva ellittiche

Windows 10, versione 1507 e Windows Server 2016 aggiungere la configurazione di criteri di gruppo per le curve ellittiche in configurazione Computer > modelli amministrativi > rete > Impostazioni di configurazione di SSL. Elenco degli ordini curva ECC specifica l'ordine in cui sono preferibili curve ellittiche nonché Abilita curve supportate che non sono abilitate. 
 
Aggiunta del supporto per le curve ellittiche seguenti:

- BrainpoolP256r1 (RFC 7027) in Windows 10, versione 1507 e Windows Server 2016
- BrainpoolP384r1 (RFC 7027) in Windows 10, versione 1507 e Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) in Windows 10, versione 1507 e Windows Server 2016
- Curve25519 (RFC draft-ietf-tls-curve25519) in Windows 10, versione 1607 e Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Supporto a livello di invio per SealMessage & UnsealMessage

Windows 10, versione 1507 e Windows Server 2016 è possibile aggiungere il supporto per SealMessage/UnsealMessage a livello di invio.

## <a name="dtls-12"></a>DTLS 1.2

Windows 10, versione 1607 e Windows Server 2016 è possibile aggiungere il supporto per 1.2 DTLS (RFC 6347).

## <a name="httpsys-thread-pool"></a>HTTP. Pool di thread SYS 

Windows 10, versione 1607 e Windows Server 2016 è possibile aggiungere la configurazione del Registro di sistema delle dimensioni del pool di thread utilizzati per gestire gli handshake di TLS per HTTP. SYS.

Percorso del Registro di sistema: 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

Per specificare una dimensione del pool massimo di thread per core della CPU, creare un **MaxAsyncWorkerThreadsPerCpu** voce. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD per le dimensioni desiderate. Se non è configurato, il valore massimo è 2 thread per core della CPU.

## <a name="next-protocol-negotiation-npn-support"></a>Supporto successivo protocollo negoziazione (NPN)

A partire da Windows 10 versione 1703, successivo protocollo di negoziazione (NPN) è stata rimossa e non è più supportata.

## <a name="pre-shared-key-psk"></a>Chiave precondivisa (PSK)

Windows 10, versione 1607 e Windows Server 2016 è possibile aggiungere il supporto per l'algoritmo di scambio delle chiavi PSK (RFC 4279).

Aggiunta del supporto per i pacchetti di crittografia PSK seguenti:

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_CBC_SHA384(RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Ripresa della sessione senza miglioramenti delle prestazioni sul lato server dello stato lato Server

Windows 10, versione 1507 e Windows Server 2016 fornisce superiori del 30% delle riprese della sessione al secondo con i ticket di sessione, rispetto a Windows Server 2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hash di sessione e l'estensione di segreto Master estesa

Windows 10, versione 1507 e Windows Server 2016 è possibile aggiungere il supporto per RFC 7627: Hash di sessione Layer Security (TLS) del trasporto ed esteso Master Secret estensione.

A causa di questa modifica, Windows 10 e Windows Server 2016 richiede 3rd party [provider CNG SSL](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx) gli aggiornamenti per supportare NCRYPT_SSL_INTERFACE_VERSION_3 e per descrivere questa nuova interfaccia.


## <a name="ssl-support"></a>Supporto SSL

A partire da Windows 10, versione 1607 e Windows Server 2016, il client TLS e il server SSL 3.0 è disabilitato per impostazione predefinita. Ciò significa che, a meno che l'applicazione o servizio in modo specifico richiede SSL 3.0 tramite l'interfaccia SSPI, il client verrà mai offerta o accettare SSL 3.0 e il server non selezionerà mai SSL 3.0.

A partire da Windows 10 versione 1607 e Windows Server 2016, SSL 2.0 è stata rimossa e non è più supportata.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Modifiche per la conformità di Windows TLS ai requisiti di TLS 1.2 per le connessioni con client TLS non conformi

In TLS 1.2, il client utilizza il [estensione "signature_algorithms"](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) per indicare al server quali coppie di algoritmi di firma/hash possono essere utilizzate nelle firme digitali (ad esempio, i certificati del server e scambio di chiavi server). RFC TLS 1.2 richiede inoltre che il messaggio di certificato server rispettano "signature_algorithms" estensione:

"Se il client fornito un'estensione"signature_algorithms", quindi tutti i certificati forniti dal server devono essere firmati da una coppia di algoritmo di firma/hash che viene visualizzato nell'estensione."

In pratica, alcuni client TLS di terze parti non conformi a RFC di TLS 1.2 e hanno esito negativo per includere tutti la firma e l'hashing coppie di algoritmi sono disposti ad accettare l'estensione "signature_algorithms" oppure omettere completamente l'estensione (quest'ultimo indica a il server che il client supporta solo SHA1 con RSA, DSA o ECDSA).

Un server TLS dispone solo di un certificato configurato per ogni endpoint, che significa che il server non può sempre fornire un certificato che soddisfa i requisiti del client.

Prima di Windows 10 e Windows Server 2016, lo stack TLS Windows scrupolosamente a TLS 1.2 requisiti RFC, causando errori di connessione con il client TLS non conforme di RFC e problemi di interoperabilità. In Windows 10 e Windows Server 2016, i vincoli sono flessibili e il server può inviare un certificato che non è conforme alla RFC TLS 1.2, se che è l'unica opzione disponibile del server. I client possono quindi continuare o terminare l'handshake.

Durante la convalida server e i certificati client, lo stack TLS Windows è strettamente conforme a RFC TLS 1.2 e consente solo gli algoritmi di hash e firma negoziato i certificati server e client.


