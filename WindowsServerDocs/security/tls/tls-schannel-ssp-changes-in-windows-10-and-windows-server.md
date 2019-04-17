---
title: TLS (SSP Schannel)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 6e1a229bfc7063597f8994c71bbaec5637d084a4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Modifiche a TLS (SSP Schannel) in Windows 10 e Windows Server 2016

>Si applica a: Windows Server 2016 e Windows 10

## <a name="cipher-suite-changes"></a>Modifiche di crittografia Suite

Windows 10 versione 1511 e Windows Server 2016 è possibile aggiungere il supporto per la configurazione dell'ordine dei pacchetti di crittografia utilizzando Gestione dispositivi mobili (MDM).

Per le modifiche nell'ordine di priorità suite crittografia, vedere [pacchetti di crittografia in Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).

Aggiunta del supporto per i pacchetti di crittografia seguenti:

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) in Windows 10 versione 1507 e Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) in Windows 10 versione 1507 e Windows Server 2016

DisabledByDefault modificare per i pacchetti di crittografia seguenti:

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) in Windows 10, versione 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) in Windows 10, versione 1703
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) in Windows 10, versione 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) in Windows 10, versione 1703
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) in Windows 10, versione 1703
- In Windows 10, versione 1709 TLS_RSA_WITH_RC4_128_SHA
- In Windows 10, versione 1709 TLS_RSA_WITH_RC4_128_MD5

A partire da Windows 10 versione 1507 e Windows Server 2016, sono supportati i certificati SHA 512 per impostazione predefinita.

### <a name="rsa-key-changes"></a>Modifiche chiave RSA

Windows 10 versione 1507 e Windows Server 2016 aggiungere opzioni di configurazione del Registro di sistema per le dimensioni delle chiavi RSA client.

Per ulteriori informazioni, vedere [KeyExchangeAlgorithm - le dimensioni delle chiavi RSA Client](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Modifiche principali Diffie-Hellman

Windows 10 versione 1507 e Windows Server 2016 aggiungere opzioni di configurazione del Registro di sistema per le dimensioni delle chiavi Diffie-Hellman.

Per ulteriori informazioni, vedere [KeyExchangeAlgorithm - le dimensioni delle chiavi Diffie-Hellman](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="schusestrongcrypto-option-changes"></a>Modifiche delle opzioni SCH_USE_STRONG_CRYPTO

Con Windows 10 versione 1507 e Windows Server 2016, [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx) opzione ora Disabilita NULL, MD5, DES e crittografie a esportare.

## <a name="elliptical-curve-changes"></a>Modifiche apportate a curva ellittiche

Windows 10 versione 1507 e Windows Server 2016 aggiungere la configurazione di criteri di gruppo per curve ellittiche definite in configurazione Computer > modelli amministrativi > rete > Impostazioni di configurazione di SSL. Elenco di ECC curva specifica l'ordine in cui sono preferibili curve ellittiche definite, nonché consente curve supportate che non sono abilitate. 
 
Aggiunta del supporto per elliptical curve seguenti:

- BrainpoolP256r1 (RFC 7027) in Windows 10 versione 1507 e Windows Server 2016
- BrainpoolP384r1 (RFC 7027) in Windows 10 versione 1507 e Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) in Windows 10 versione 1507 e Windows Server 2016
- Curve25519 (RFC bozza-ietf-tls-curve25519) in Windows 10, versione 1607 e Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Supporto del livello di invio per SealMessage & UnsealMessage

Windows 10 versione 1507 e Windows Server 2016 è possibile aggiungere il supporto per SealMessage/UnsealMessage a livello di invio.

## <a name="dtls-12"></a>DTLS 1.2

Windows 10 versione 1607 e Windows Server 2016 aggiungere il supporto per 1.2 DTLS (RFC 6347).

## <a name="httpsys-thread-pool"></a>PROTOCOLLO HTTP. Pool di thread SYS 

Windows 10 versione 1607 e Windows Server 2016 è possibile aggiungere la configurazione del Registro di sistema della dimensione del pool di thread utilizzato per gestire gli handshake TLS per HTTP. SYS.

Percorso del Registro di sistema: 

HKLM\System\CurrentControlSet\Control\LSA.

Per specificare una dimensione del pool massimo di thread per ogni core della CPU, creare un **MaxAsyncWorkerThreadsPerCpu** voce. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD sulla dimensione desiderata. Se non è configurato, il valore massimo è 2 thread per ogni core CPU.

## <a name="next-protocol-negotiation-npn-support"></a>Supporto di negoziazione protocollo (NPN) Avanti

A partire da Windows 10 versione 1703, Avanti protocollo di negoziazione (NPN) è stato rimosso e non è più supportata.

## <a name="pre-shared-key-psk"></a>Chiave precondivisa (PSK)

Windows 10 versione 1607 e Windows Server 2016 aggiungere il supporto per l'algoritmo di scambio di chiave PSK (RFC 4279).

Aggiunta del supporto per i pacchetti di crittografia PSK seguenti:

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_CBC_SHA384(RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) in Windows 10, versione 1607 e Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Ripresa della sessione senza miglioramenti delle prestazioni sul lato server dello stato sul lato Server

Windows 10 versione 1507 e Windows Server 2016 fornire più di 30% delle riprese della sessione al secondo i ticket di sessione rispetto a Windows Server 2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hash della sessione e l'estensione di segreto Master estesa

Aggiungere il supporto per RFC 7627 Windows 10 versione 1507 e Windows Server 2016: Hash sessione Transport Layer Security (TLS) ed esteso estensione segreto Master.

A causa di questa modifica, Windows 10 e Windows Server 2016 richiede 3rd party [provider CNG SSL](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx) gli aggiornamenti per il supporto NCRYPT_SSL_INTERFACE_VERSION_3 e per descrivere questa nuova interfaccia.


## <a name="ssl-support"></a>Supporto SSL

A partire da Windows 10 versione 1607 e Windows Server 2016, il client TLS e il server SSL 3.0 è disabilitato per impostazione predefinita. Ciò significa che, a meno che l'applicazione o servizio in modo specifico richiede SSL 3.0 tramite SSPI, il client non verrà mai offrono o accettare SSL 3.0 e il server non verrà mai selezionare SSL 3.0.

A partire da Windows 10 versione 1607 e Windows Server 2016, SSL 2.0 è stato rimosso e non è più supportata.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Modifiche per la conformità Windows TLS ai requisiti di TLS 1.2 per le connessioni con client TLS non conforme

In TLS 1.2, il client utilizza il [estensione "signature_algorithms"](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) per indicare al server le coppie di algoritmi di firma/hash possono essere usate in firme digitali (ad esempio, i certificati del server e scambio di chiave server). RFC TLS 1.2 richiede inoltre che il messaggio di certificato server rispetta estensione "signature_algorithms":

"Se il client fornito un'estensione"signature_algorithms", quindi tutti i certificati forniti dal server devono essere firmati da una coppia di algoritmo di firma/hash che viene visualizzato in tale estensione."

In pratica, alcuni client TLS di terze parti non conformi a RFC di TLS 1.2 e hanno esito negativo per includere tutte le firme e hash di coppie di algoritmi di cui si è disposti ad accettare l'estensione "signature_algorithms" oppure omettere l'estensione del tutto (quest'ultimo indica al server che il client supporta solo SHA1 con RSA, DSA o ECDSA).

Un server TLS ha spesso solo un certificato configurato per ogni endpoint, ovvero che il server non è in grado di fornire sempre un certificato che soddisfa i requisiti del client.

Prima di Windows 10 e Windows Server 2016, lo stack di Windows TLS scrupolosamente di TLS 1.2 requisiti RFC, causando errori di connessione con i client TLS non conforme RFC e problemi di interoperabilità. In Windows 10 e Windows Server 2016, i vincoli sono meno rigide e il server può inviare un certificato che non è conforme alle RFC TLS 1.2, se questa opzione solo del server. Il client può quindi continuare o terminare l'handshake.

Durante la convalida dei certificati server e client, lo stack di Windows TLS strettamente sia conforme a RFC TLS 1.2 e consente solo la firma negoziato e algoritmi hash i certificati server e client.


