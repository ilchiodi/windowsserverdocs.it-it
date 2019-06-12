---
title: Le impostazioni del Registro di sistema Layer Security (TLS) di trasporto
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 02/28/2019
ms.openlocfilehash: 32068319aae7545675e126eed6e1ab4c914bcbcf
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812641"
---
# <a name="transport-layer-security-tls-registry-settings"></a>Le impostazioni del Registro di sistema Layer Security (TLS) di trasporto

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10

Questo argomento destinato ai professionisti IT di riferimento contiene informazioni sulle impostazioni del Registro di sistema supportate per l'implementazione Windows del protocollo Transport Layer Security (TLS) e il protocollo Secure Sockets Layer (SSL) grazie al supporto sicurezza Schannel Provider (SSP). Le sottochiavi del Registro di sistema e le voci illustrate in questo argomento della Guida amministrare e risolvere i problemi di SSP Schannel, in particolare i protocolli TLS e SSL. 

> [!CAUTION]
> Queste informazioni vengono fornite come riferimento utilizzabile durante la risoluzione dei problemi o quando si verifica che le impostazioni obbligatorie siano applicate.
> È consigliabile non modificare direttamente il Registro di sistema, a meno che non ci siano altre alternative.
> Le modifiche al Registro di sistema non vengono convalidate dall'editor del Registro di sistema o dal sistema operativo Windows prima di essere applicate.
> Di conseguenza, è possibile che vengano archiviati valori non corretti, che possono causare errori irreversibili del sistema.
> Se possibile, invece di modificare direttamente il Registro di sistema, usare Criteri di gruppo o altri strumenti di Windows, come Microsoft Management Console (MMC), per eseguire queste attività.
> Se è necessario modificare il Registro di sistema, usare la massima cautela.

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Il valore predefinito consiste nel supporto di tutti e quattro i metodi di mapping dei certificati elencati di seguito.

Quando un'applicazione server richiede l'autenticazione client, Schannel prova automaticamente a eseguire il mapping del certificato fornito dal computer client a un account utente. È possibile autenticare gli utenti che accedono con un certificato client creando mapping che mettono in relazione le informazioni sul certificato con un account utente di Windows. Dopo avere creato e abilitato il mapping di un certificato, ogni volta che un client presenta un certificato client, l'applicazione server associa automaticamente l'utente con l'account utente di Windows corretto.

Nella maggior parte dei casi, il mapping di un certificato a un account utente viene eseguito in uno dei due modi seguenti: 

- Un singolo certificato viene mappato a un singolo account utente (mapping uno a uno).
- Più certificati vengono mappati a un account utente (mapping molti a uno).

Per impostazione predefinita, il provider Schannel usa i quattro metodi di mapping dei certificati seguenti, in ordine di preferenza:

1. Mapping dei certificati Kerberos Service-For-User (S4U)
2. Mapping del nome dell'entità utente
3. Mapping uno a uno (detto anche mapping soggetto/autorità di certificazione)
4. Mapping molti a uno

Versioni applicabili: come indicato nell'elenco **Si applica a** all'inizio di questo argomento.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Ciphers

Crittografia TLS/SSL deve essere controllata configurando l'ordine dei pacchetti di crittografia. Per informazioni dettagliate, vedere [configurazione TLS Cipher Suite ordine](manage-tls.md#configuring-tls-cipher-suite-order).

Per informazioni sull'ordine di gruppi di pacchetti di crittografia predefiniti usati da SSP Schannel, vedere [pacchetti di crittografia in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 

## <a name="ciphersuites"></a>CipherSuites

Configurazione di pacchetti di crittografia TLS/SSL deve essere eseguita tramite criteri di gruppo, MDM o PowerShell, vedere [configurazione TLS Cipher Suite ordine](manage-tls.md#configuring-tls-cipher-suite-order) per informazioni dettagliate.

Per informazioni sull'ordine di gruppi di pacchetti di crittografia predefiniti usati da SSP Schannel, vedere [pacchetti di crittografia in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 


## <a name="clientcachetime"></a>ClientCacheTime

Questa voce controlla l'intervallo di tempo, in millisecondi, che il sistema operativo impiega per la scadenza delle voci della cache sul lato client. Il valore 0 disattiva la memorizzazione nella cache per la connessione sicura. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. 

La prima volta che un client si connette a un server tramite SSP Schannel, viene eseguito un handshake TLS/SSL completo. Al termine, master secret, pacchetto di crittografia e certificati vengono archiviati nella cache della sessione sui rispettivi client e server.

A partire da Windows Server 2008 e Windows Vista, il tempo della cache del client predefinito è 10 ore.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempi di memorizzazione nella cache client predefiniti

## <a name="enableocspstaplingforsni"></a>EnableOcspStaplingForSni

Graffatura online certificato OCSP (Status Protocol) consente a un server web, ad esempio Internet Information Services (IIS), per fornire lo stato corrente di revoca dei certificati server quando invia il certificato del server a un client durante l'handshake TLS. Questa funzionalità riduce il carico sui server OCSP perché il server web può memorizzare nella cache lo stato corrente di OCSP del certificato del server e inviarlo a più client web. Senza questa funzionalità, ogni client web tentava di recuperare lo stato corrente di OCSP del certificato del server dal server OCSP. Questo potrebbe generare un carico elevato in tale server OCSP. 

Oltre a IIS, i servizi web tramite HTTP. sys possono trarre vantaggio da questa impostazione, tra cui Active Directory Federation Services (ADFS) e Proxy applicazione Web (WAP). 

Per impostazione predefinita, il supporto OCSP è abilitato per i siti Web IIS con associazione semplice protetta (SSL/TLS). Tuttavia, questo supporto non è abilitato per impostazione predefinita se il sito Web IIS utilizza uno o entrambi i tipi di binding protetto (SSL/TLS) seguenti:
- Richiedi indicazione nome Server
- Usa archivio certificati centralizzato

In questo caso, la risposta hello del server durante l'handshake TLS non include uno stato di graffatura OCSP per impostazione predefinita. Questo comportamento migliora le prestazioni: L'implementazione di graffatura Windows OCSP adattabile a centinaia di certificati del server. Dal momento che connessioni SNI e CCS abilitare IIS per la scalabilità a migliaia di siti Web che hanno potenzialmente migliaia di certificati del server, impostare questo comportamento deve essere abilitata per impostazione predefinita può causare problemi di prestazioni.

Versioni applicabili: Tutte le versioni a partire da Windows Server 2012 e Windows 8. 

Percorso del Registro di sistema: [hkey_local_machine\system\currentcontrolset\control\securityproviders\schannel.]

Aggiungere la chiave seguente:

"EnableOcspStaplingForSni"=dword:00000001

Per disabilitare, impostare il valore DWORD su 0:

"EnableOcspStaplingForSni"=dword:00000000

> [!NOTE] 
> L'abilitazione di questa chiave del Registro di sistema ha un impatto potenziale sulle prestazioni.

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Questa voce controlla la conformità con FIPS (Federal Information Processing Standard). Il valore predefinito è 0.

Versioni applicabili: Tutte le versioni a partire da Windows Server 2012 e Windows 8. 

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\LSA

Pacchetti di crittografia FIPS per Windows Server: Visualizzare [pacchetti di crittografia e i protocolli supportati in SSP Schannel](https://technet.microsoft.com/library/dn786419.aspx).

## <a name="hashes"></a>Hashes

Gli algoritmi di hash TLS/SSL devono essere controllati configurando l'ordine dei pacchetti di crittografia. Visualizzare [configurazione TLS Cipher Suite ordine](manage-tls.md#configuring-tls-cipher-suite-order) per informazioni dettagliate.

## <a name="issuercachesize"></a>IssuerCacheSize

Questa voce controlla le dimensioni della cache dell'autorità di certificazione ed è usata con il mapping dell'autorità di certificazione. SSP Schannel prova a eseguire il mapping di tutte le autorità di certificazione nella catena di certificati del client, non solo l'autorità di certificazione diretta del certificato client. Quando non viene eseguito il mapping delle autorità di certificazione a un account, un caso tipico, il server potrebbe provare a eseguire ripetutamente il mapping dello stesso nome dell'autorità di certificazione, centinaia di volte al secondo. 

Per evitare questo problema, nel server è disponibile una cache negativa, quindi se il mapping di un nome dell'autorità di certificazione a un account non viene eseguito, viene aggiunto alla cache e SSP Schannel non proverà a eseguire di nuovo il mapping di quel nome finché la voce della cache non scadrà. Questa voce del Registro di sistema specifica le dimensioni della cache. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Il valore predefinito è 100. 

Versioni applicabili: Tutte le versioni a partire da Windows Server 2008 e Windows Vista.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Questa voce controlla la durata dell'intervallo di timeout della cache in millisecondi. SSP Schannel prova a eseguire il mapping di tutte le autorità di certificazione nella catena di certificati del client, non solo l'autorità di certificazione diretta del certificato client. Se non viene eseguito il mapping delle autorità di certificazione a un account, un caso tipico, il server potrebbe provare a eseguire ripetutamente il mapping dello stesso nome dell'autorità di certificazione, centinaia di volte al secondo.

Per evitare questo problema, nel server è disponibile una cache negativa, quindi se il mapping di un nome dell'autorità di certificazione a un account non viene eseguito, viene aggiunto alla cache e SSP Schannel non proverà a eseguire di nuovo il mapping di quel nome finché la voce della cache non scadrà. Questa cache viene mantenuta ai fini delle prestazioni, per evitare che il sistema continui a provare a eseguire il mapping delle stesse autorità di certificazione. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Il valore predefinito è 10 minuti.

Versioni applicabili: Tutte le versioni a partire da Windows Server 2008 e Windows Vista.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>Le dimensioni della chiave Client RSA KeyExchangeAlgorithm-

Questa voce controlla le dimensioni della chiave RSA client. 

Uso di algoritmi di scambio delle chiavi deve essere controllato configurando l'ordine dei pacchetti di crittografia.

Aggiunta in Windows 10, versione 1507 e Windows Server 2016.

Percorso del Registro di sistema: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Per specificare una lunghezza di intervallo minimo supportato di RSA bit per la chiave per il client TLS, creare un **ClientMinKeyBitLength** voce. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderate. Se non è configurato, 1024 bit sarà il valore minimo. 

Per specificare una lunghezza di intervallo massimo supportato di RSA bit per la chiave per il client TLS, creare un **ClientMaxKeyBitLength** voce. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderate. Se non è configurato, non viene applicata una massima.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm - dimensioni della chiave Diffie-Hellman

Questa voce controlla le dimensioni della chiave Diffie-Hellman. 

Uso di algoritmi di scambio delle chiavi deve essere controllato configurando l'ordine dei pacchetti di crittografia.

Aggiunta in Windows 10, versione 1507 e Windows Server 2016.

Percorso del Registro di sistema: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Per specificare una lunghezza di chiave a bit minimi supportati intervallo di Diffie-Helman per il client TLS, creare un **ClientMinKeyBitLength** voce. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderate. Se non è configurato, 1024 bit sarà il valore minimo. 
 
Per specificare una lunghezza di massimo supportato intervallo di Diffie-Helman bit per la chiave per il client TLS, creare un **ClientMaxKeyBitLength** voce. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderate. Se non è configurato, non viene applicata una massima. 
 
Per specificare la lunghezza in bit chiave Diffie-Helman per il valore predefinito del server TLS, creare un **ServerMinKeyBitLength** voce. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderate. Se non è configurato, 2048 bit sarà il valore predefinito. 

## <a name="maximumcachesize"></a>MaximumCacheSize

Questa voce controlla il numero massimo di elementi della cache. L'impostazione di Setting MaximumCacheSize su 0 disabilita la cache della sessione sul lato server e impedisce la riconnessione. L'aumento del valore di MaximumCacheSize oltre i valori predefiniti causa il consumo di memoria aggiuntiva da parte di Lsass.exe. Ogni elemento della cache della sessione richiede in genere da 2 a 4 KB di memoria. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Il valore predefinito è 20.000 elementi. 

Versioni applicabili: Tutte le versioni a partire da Windows Server 2008 e Windows Vista.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Messaging-frammentazione analisi

________________________________________
Questa voce controlla le dimensioni massime consentite di frammentazione dei messaggi di handshake TLS che verranno accettati. Non verranno accettati messaggi maggiori di quelle consentite e l'handshake TLS avrà esito negativo. Queste voci non esistono nel Registro di sistema per impostazione predefinita. 

Quando si imposta il valore per valore 0x0, frammentato i messaggi non vengono elaborati e causano l'handshake TLS esito negativo. In questo modo i client TLS o i server nel computer corrente non è conforme alla RFC TLS. 

Numero massimo di dimensioni possono essere aumentate fino a 2 ^ 24-1 byte. Che consente a un client o server leggere e archiviare grandi quantità di dati non verificati da una rete non è una buona idea e utilizzo di memoria aggiuntiva per ogni contesto di sicurezza. 

Aggiunta in Windows 7 e Windows Server 2008 R2.
È disponibile un aggiornamento che consente a Internet Explorer in Windows XP, Windows Vista o in Windows Server 2008 per analizzare i messaggi di handshake TLS/SSL frammentati.

Percorso del Registro di sistema: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Per specificare una dimensione massima consentita dei messaggi di handshake TLS frammentati che accetterà il client TLS, creare un **MessageLimitClient** voce. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderate. Se non è configurato, il valore predefinito sarà 0x8000 byte. 

Per specificare una dimensione massima consentita dei messaggi di handshake TLS frammentati che il server TLS accetterà quando è presente alcuna autenticazione del client, creare un **MessageLimitServer** voce. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderate. Se non è configurato, il valore predefinito sarà 0x4000 byte. 

Per specificare una dimensione massima consentita dei messaggi di handshake TLS frammentati che il server TLS accetterà quando è presente l'autenticazione client, creare un **MessageLimitServerClientAuth** voce. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderate. Se non è configurato, il valore predefinito sarà 0x8000 byte. 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Questa voce controlla il flag usato quando viene inviato l'elenco delle autorità di certificazione attendibili. Nel caso di server che considerano attendibili centinaia di autorità di certificazione per l'autenticazione client, il server non potrà inviare tutte queste autorità di certificazione al computer client quando richiede l'autorizzazione client a causa del loro numero eccessivo. In questa situazione, è possibile impostare questa chiave del Registro di sistema e, invece di inviare un elenco parziale, SSP Schannel non invierà alcun elenco al client.

Il mancato invio di un elenco di autorità di certificazione attendibili potrebbe avere un impatto su ciò che il client invia quando viene richiesto un certificato client. Ad esempio, quando Internet Explorer riceve una richiesta di autenticazione client, visualizza solo i certificati client concatenati a una delle autorità di certificazione inviata dal server. Se il server non ha inviato un elenco, Internet Explorer visualizza tutti i certificati client installati nel client. 

Questo comportamento potrebbe risultare utile. Ad esempio, quando gli ambienti PKI includono certificati incrociati, i certificati client e server non avranno la stessa CA radice, quindi Internet Explorer non potrà scegliere un certificato concatenato a una delle CA del server. Se si configura il server in modo che non invii un elenco di autorità di certificazione attendibili, Internet Explorer invierà tutti i propri certificati.

Questa voce non è disponibile nel Registro di sistema per impostazione predefinita.

Comportamento di invio dell'elenco di autorità di certificazione attendibili predefinito

| Versione di Windows | Time |
|-----------------|------|
| Windows Server 2012 e Windows 8 e versioni successive | FALSE |
| Windows Server 2008 R2 e Windows 7 e versioni precedenti | TRUE |

Versioni applicabili: Tutte le versioni a partire da Windows Server 2008 e Windows Vista.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

Questa voce controlla l'intervallo di tempo, in millisecondi, che il sistema operativo impiega per la scadenza delle voci della cache sul lato server. Un valore 0 disabilita la cache della sessione sul lato server e impedisce la riconnessione. L'aumento del valore di ServerCacheTime oltre i valori predefiniti causa il consumo di memoria aggiuntiva da parte di Lsass.exe. Ogni elemento della cache di sessione richiede in genere da 2 a 4 KB di memoria. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. 

Versioni applicabili: Tutte le versioni a partire da Windows Server 2008 e Windows Vista.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempi di cache server predefiniti: 10 ore

## <a name="ssl-20"></a>SSL 2.0

Questa sottochiave controlla l'uso di SSL 2.0.

A partire da Windows 10, versione 1607 e Windows Server 2016, SSL 2.0 è stata rimossa e non è più supportata.
Per le impostazioni predefinite un SSL 2.0, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo SSL 2.0, creare un **abilitato** voce nella sottochiave Client o Server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi di SSL 2.0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di SSL 2.0 nel client SSL. |
| Server | Controlla l'uso di SSL 2.0 nel server SSL. |

Per disabilitare SSL 2.0 per client o server, modificare il valore DWORD in 0. Se un'app SSPI richiede l'utilizzo di SSL 2.0, verranno rifiutate. 

Per disabilitare SSL 2.0 per impostazione predefinita, creare un **DisabledByDefault** voce e modifica il valore DWORD valore su 1. Se un esplicitamente app SSPI richiede l'utilizzo di SSL 2.0, può essere negoziato. 

Nell'esempio seguente viene disabilitato nel Registro di sistema SSL 2.0:

![SSL 2.0 disabilitato](images/ssl-2-registry-setting.png)


## <a name="ssl-30"></a>SSL 3.0

Questa sottochiave controlla l'uso di SSL 3.0.

A partire da Windows 10, versione 1607 e Windows Server 2016, SSL 3.0 è stato disabilitato per impostazione predefinita. Per le impostazioni predefinite di SSL 3.0, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo SSL 3.0, creare un **abilitato** voce nella sottochiave Client o Server, come descritto nella tabella seguente.  
Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi di SSL 3.0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di SSL 3.0 nel client SSL. |
| Server | Controlla l'uso di SSL 3.0 nel server SSL. |

Per disabilitare SSL 3.0 per client o server, modificare il valore DWORD in 0.
Se un'app SSPI richiede l'utilizzo di SSL 3.0, verranno rifiutate. 

Per disabilitare SSL 3.0 per impostazione predefinita, creare un **DisabledByDefault** voce e modifica il valore DWORD valore su 1. Se un'app SSPI richiede in modo esplicito l'utilizzo di SSL 3.0, potrebbe essere negoziato. 

L'esempio seguente illustra SSL 3.0 sono disabilitati nel Registro di sistema:

![SSL 3.0 sono disabilitati](images/ssl-3-registry-setting.png)

## <a name="tls-10"></a>TLS 1.0

Questa sottochiave controlla l'uso di TLS 1.0.

Per le impostazioni predefinite di TLS 1.0, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo TLS 1.0, creare un **abilitato** voce nella sottochiave Client o Server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi di TLS 1.0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di TLS 1.0 nel client TLS. |
| Server | Controlla l'uso di TLS 1.0 nel server di TLS. |

Per disabilitare TLS 1.0 per il client o server, modificare il valore DWORD in 0.
Se un'app SSPI richiede di usare TLS 1.0, verranno rifiutate. 

Per disabilitare TLS 1.0 per impostazione predefinita, creare un **DisabledByDefault** voce e modifica il valore DWORD valore su 1. Se un'app SSPI richiede in modo esplicito a usano TLS 1.0, potrebbe essere negoziato. 

L'esempio seguente illustra TLS 1.0 disabilitata nel Registro di sistema:

![TLS 1.0 disabilitata](images/tls-registry-setting.png)

## <a name="tls-11"></a>TLS 1.1

Questa sottochiave controlla l'uso di TLS 1.1.

Per le impostazioni predefinite di TLS 1.1, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo TLS 1.1, creare un **abilitato** voce nella sottochiave Client o Server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi di TLS 1.1

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di TLS 1.1 nel client TLS. |
| Server | Controlla l'uso di TLS 1.1 nel server di TLS. |

Per disabilitare TLS 1.1 per il client o server, modificare il valore DWORD in 0.
Se un'app SSPI richiede usi TLS 1.1, verranno rifiutate. 

Per disabilitare TLS 1.1 per impostazione predefinita, creare un **DisabledByDefault** voce e modifica il valore DWORD valore su 1. Se un'app SSPI richiede in modo esplicito a usi TLS 1.1, potrebbe essere negoziato. 

Nell'esempio seguente viene disabilitato nel Registro di sistema di TLS 1.1:

![TLS 1.1 disabilitato](images/tls-11-registry-setting.png)

## <a name="tls-12"></a>TLS 1.2

Questa sottochiave controlla l'uso di TLS 1.2.

Per le impostazioni predefinite di TLS 1.2, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo TLS 1.2, creare un **abilitato** voce nella sottochiave Client o Server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi di TLS 1.2

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di TLS 1.2 nel client TLS. |
| Server | Controlla l'uso di TLS 1.2 nel server di TLS. |

Per disabilitare TLS 1.2 per client o server, modificare il valore DWORD in 0.
Se un'app SSPI richiede di usare TLS 1.2, verranno rifiutate. 

Per disabilitare TLS 1.2 per impostazione predefinita, creare un **DisabledByDefault** voce e modifica il valore DWORD valore su 1. Se un'app SSPI richiede in modo esplicito per usare TLS 1.2, potrebbe essere negoziato. 

Nell'esempio seguente viene disabilitato nel Registro di sistema di TLS 1.2:

![TLS 1.2 disabilitato](images/tls-12-registry-setting.png)

## <a name="dtls-10"></a>DTLS 1.0

Questa sottochiave controlla l'uso di DTLS 1.0.

Per le impostazioni predefinite di DTLS 1.0, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo DTLS 1.0, creare un **abilitato** voce nella sottochiave Client o Server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi di DTLS 1.0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di DTLS 1.0 nel client DTLS. |
| Server | Controlla l'uso di DTLS 1.0 nel server di DTLS. |

Per disabilitare DTLS 1.0 per il client o server, modificare il valore DWORD in 0.
Se un'app SSPI richiede di usare DTLS 1.0, verranno rifiutate. 

Per disabilitare DTLS 1.0 per impostazione predefinita, creare un **DisabledByDefault** voce e modifica il valore DWORD valore su 1. Se un'app SSPI richiede esplicitamente a usare DTLS 1.0, potrebbe essere negoziato. 

Nell'esempio seguente mostra DTLS 1.0 disabilitata nel Registro di sistema:

![DTLS 1.0 disabilitata](images/dtls-10-registry-setting.png)

## <a name="dtls-12"></a>DTLS 1.2

Questa sottochiave controlla l'uso di DTLS 1.2.

Per le impostazioni predefinite di DTLS 1.2, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo DTLS 1.2, creare un **abilitato** voce nella sottochiave Client o Server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi di DTLS 1.2

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di DTLS 1.2 nel client DTLS. |
| Server | Controlla l'uso di DTLS 1.2 nel server di DTLS. |


Per disabilitare DTLS 1.2 per client o server, modificare il valore DWORD su 0.
Se un'app SSPI richiede di usare DTLS 1.0, verranno rifiutate. 

Per disabilitare DTLS 1.2 per impostazione predefinita, creare un **DisabledByDefault** voce e modifica il valore DWORD valore su 1. Se un'app SSPI richiede esplicitamente a usare DTLS 1.2, potrebbe essere negoziato. 

L'esempio seguente illustra 1.1 DTLS disabilitati nel Registro di sistema:

![1.1 DTLS disabilitato](images/dtls-11-registry-setting.png)


