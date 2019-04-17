---
title: Le impostazioni del Registro di sistema Layer Security (TLS) di trasporto
description: Protezione di Windows Server
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
ms.date: 08/07/2017
ms.openlocfilehash: 8ccfacc367a5d32438bebf22798479a07f6cbdfc
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="transport-layer-security-tls-registry-settings"></a>Le impostazioni del Registro di sistema Layer Security (TLS) di trasporto

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista

Questo argomento di riferimento per i professionisti IT contiene informazioni sulle impostazioni del Registro di sistema supportate per l'implementazione Windows del protocollo Transport Layer Security (TLS) e il protocollo Secure Sockets Layer (SSL) tramite Schannel Security Support Provider (SSP). Le sottochiavi del Registro di sistema e le voci trattate in questo argomento della Guida in linea amministrare e risolvere i problemi di SSP Schannel, in particolare i protocolli SSL e TLS. 

>[!Caution]
>Queste informazioni vengono fornite come riferimento da utilizzare quando si esegue la risoluzione dei problemi o verificare che siano applicate le impostazioni necessarie. È consigliabile che non modificare direttamente il Registro di sistema a meno che non vi è alcuna altra alternativa.
>Le modifiche al Registro di sistema non vengono convalidate dall'Editor del Registro di sistema o dal sistema operativo Windows prima che vengano applicate. Di conseguenza, possono essere archiviati valori non corretti, e ciò può causare errori irreversibili nel sistema. Se possibile, invece di modificare il Registro di sistema direttamente, utilizzare criteri di gruppo o altri strumenti di Windows, ad esempio Microsoft Management Console (MMC) per eseguire attività. Se è necessario modificare il Registro di sistema, usare estrema cautela. 

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

Questa voce non esiste nel Registro di sistema per impostazione predefinita. Il valore predefinito è che tutti i metodi quattro mapping dei certificati elencati di seguito sono supportati.

Quando un'applicazione server richiede l'autenticazione client, Schannel prova automaticamente a eseguire il mapping del certificato fornito dal computer client a un account utente. È possibile autenticare gli utenti che accedono con un certificato client creando mapping che mettono in relazione le informazioni del certificato a un account utente di Windows. Dopo avere creato e attiva il mapping di un certificato, ogni volta che un client presenta un certificato client, l'applicazione server associa automaticamente l'utente con l'account utente di Windows appropriata.

Nella maggior parte dei casi, un certificato è mappato a un account utente in uno dei due modi: 

- Un singolo certificato viene mappato a un singolo account utente (mapping uno a uno).
- Più certificati vengono mappati a un account utente (mapping molti-a-uno).

Per impostazione predefinita, il provider Schannel userà i seguenti metodi di quattro mapping dei certificati, elencati in ordine di preferenza:

1. Mapping dei certificati Kerberos service-for-user (S4U)
2. Mapping dei nomi dell'entità utente
3. Mapping uno a uno (noto anche come soggetto/autorità di certificazione mapping)
4. Mapping molti-a-uno

Versioni applicabili: come indicato nel **si applica a** elenco all'inizio di questo argomento.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Crittografia

Crittografia TLS/SSL deve essere controllati configurando l'ordine dei pacchetti di crittografia. Per ulteriori informazioni, vedere [configurazione ordine dei pacchetti di crittografia TLS](manage-tls.md#configuring-tls-cipher-suite-order).

Per informazioni sull'ordine dei pacchetti di crittografia predefinito che vengono usati da SSP Schannel, vedere [pacchetti di crittografia di TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 

##<a name="ciphersuites"></a>CipherSuites

La configurazione di pacchetti di crittografia TLS/SSL deve essere eseguita tramite criteri di gruppo, MDM o PowerShell, vedere [configurazione ordine dei pacchetti di crittografia TLS](manage-tls.md#configuring-tls-cipher-suite-order) per informazioni dettagliate.

Per informazioni sull'ordine dei pacchetti di crittografia predefinito che vengono usati da SSP Schannel, vedere [pacchetti di crittografia di TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 


## <a name="clientcachetime"></a>ClientCacheTime

Questa voce controlla la quantità di tempo che il sistema operativo impiega in millisecondi per la scadenza delle voci della cache sul lato client. Il valore 0 disattiva la memorizzazione nella cache di connessione sicura. Questa voce non esiste nel Registro di sistema per impostazione predefinita. 

La prima volta un client si connette a un server tramite SSP Schannel, completo viene eseguita l'handshake TLS/SSL. Al termine, il master secret, pacchetto di crittografia e certificati vengono archiviati nella cache della sessione sui rispettivi client e server.

A partire da Windows Server 2008 e Windows Vista, il tempo della cache del client predefinito è 10 ore.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempi di cache client predefiniti

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Questa voce controlla la conformità FIPS Federal Information Processing (). Il valore predefinito è 0.

Versioni applicabili: come indicato nel **si applica a** elenco all'inizio di questo argomento. 

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\LSA

Pacchetti di crittografia FIPS per Windows Server: vedere [supportati i pacchetti di crittografia e protocolli in SSP Schannel](https://technet.microsoft.com/library/dn786419.aspx).

## <a name="hashes"></a>Hash

Gli algoritmi hash TLS/SSL devono essere controllati configurando l'ordine dei pacchetti di crittografia. Vedere [configurazione ordine dei pacchetti di crittografia TLS](manage-tls.md#configuring-tls-cipher-suite-order) per informazioni dettagliate.

## <a name="issuercachesize"></a>IssuerCacheSize

Questa voce controlla la dimensione della cache dell'autorità di certificazione e viene utilizzato con il mapping dell'autorità di certificazione. SSP Schannel prova a eseguire il mapping di tutte le autorità emittenti nella catena di certificati del client, non solo l'autorità di certificazione diretta del certificato client. Quando non esegue il mapping delle autorità di certificazione a un account, che è il caso tipico, il server potrebbe provare a mapping dello stesso nome di autorità di certificazione più volte, centinaia di volte al secondo. 

Per evitare questo problema, il server ha una cache negativa, in modo che se un nome dell'autorità di certificazione non è mappato a un account, viene aggiunto alla cache e SSP Schannel non tenterà di mapping del nome dell'autorità di certificazione nuovamente finché non scade la voce della cache. Questa voce del Registro di sistema specifica le dimensioni della cache. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Il valore predefinito è 100. 

Versioni applicabili: come indicato nel **si applica a** elenco all'inizio di questo argomento.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Questa voce controlla la durata dell'intervallo di timeout della cache in millisecondi. SSP Schannel prova a eseguire il mapping di tutte le autorità emittenti nella catena di certificati del client, non solo l'autorità di certificazione diretta del certificato client. Nel caso in cui l'autorità di certificazione non eseguono il mapping a un account, che è il caso tipico, il server potrebbe provare a eseguire il mapping lo stesso nome di autorità di certificazione più volte, centinaia di volte al secondo.

Per evitare questo problema, il server ha una cache negativa, in modo che se un nome dell'autorità di certificazione non è mappato a un account, viene aggiunto alla cache e SSP Schannel non tenterà di mapping del nome dell'autorità di certificazione nuovamente finché non scade la voce della cache. Questa cache viene mantenuta per motivi di prestazioni, in modo che il sistema continui a provare a eseguire il mapping di autorità di certificazione stessa. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Il valore predefinito è 10 minuti.

Versioni applicabili: come indicato nel **si applica a** elenco all'inizio di questo argomento.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm - dimensioni di chiave Client RSA

Questa voce controlla le dimensioni di chiavi RSA client. 

Utilizzo degli algoritmi di scambio di chiave deve essere controllato configurando l'ordine dei pacchetti di crittografia.

Aggiunta in Windows 10 versione 1507 e Windows Server 2016.

Percorso del Registro di sistema: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Per specificare una lunghezza in bit chiave intervallo minimo supportato di RSA per il client TLS, creare un **ClientMinKeyBitLength** voce. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderato. Se non è configurato, 1024 bit sarà il requisito minimo. 

Per specificare una lunghezza in bit chiave intervallo massimo supportato di RSA per il client TLS, creare un **ClientMaxKeyBitLength** voce. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderato. Se non è configurato, non viene applicato un massimo.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm - le dimensioni delle chiavi Diffie-Hellman

Questa voce controlla le dimensioni delle chiavi Diffie-Hellman. 

Utilizzo degli algoritmi di scambio di chiave deve essere controllato configurando l'ordine dei pacchetti di crittografia.

Aggiunta in Windows 10 versione 1507 e Windows Server 2016.

Percorso del Registro di sistema: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Per specificare una lunghezza di chiave bit minimo supportato intervallo di Diffie-Helman per il client TLS, creare un **ClientMinKeyBitLength** voce. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderato. Se non è configurato, 1024 bit sarà il requisito minimo. 
 
Per specificare una lunghezza di chiave bit massima supportata intervallo di Diffie-Helman per il client TLS, creare un **ClientMaxKeyBitLength** voce. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderato. Se non è configurato, non viene applicato un massimo. 
 
Per specificare la lunghezza in bit chiave Diffie-Helman per impostazione predefinita del server TLS, creare un **ServerMinKeyBitLength** voce. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderato. Se non è configurato, pari a 2048 bit sarà il valore predefinito. 

## <a name="maximumcachesize"></a>Valore di MaximumCacheSize

Questa voce controlla il numero massimo di elementi della cache. L'impostazione di Setting MaximumCacheSize su 0 disabilita la cache della sessione sul lato server e impedisce la riconnessione. L'aumento del valore di MaximumCacheSize oltre i valori predefiniti causa Lsass.exe per l'utilizzo di memoria aggiuntiva. Ogni elemento della cache della sessione richiede in genere da 2 a 4 KB di memoria. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Il valore predefinito è 20.000 elementi. 

Versioni applicabili: come indicato nel **si applica a** elenco all'inizio di questo argomento.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Messaggistica – l'analisi del frammento

________________________________________
Questa voce controlla la dimensione massima consentita di messaggi di handshake TLS frammentati che verranno accettati. Messaggi di dimensioni maggiori rispetto alle dimensioni consentite non verranno accettati e l'handshake TLS avrà esito negativo. Queste voci non esistono nel Registro di sistema per impostazione predefinita. 

Quando si imposta il valore su 0x0, frammentati messaggi non elaborati e causeranno l'handshake TLS avere esito negativo. In questo modo il client TLS o i server nel computer in uso non è compatibile con le specifiche RFC TLS. 

Numero massimo di dimensioni possono essere aumentata fino a 2 ^ 24-1 byte. Consentendo un client o server leggere e archiviare grandi quantità di dati non verificati da una rete non è una buona idea e utilizzerà memoria aggiuntiva per ogni contesto di sicurezza. 

Aggiunta in Windows 7 e Windows Server 2008 R2.
È disponibile un aggiornamento che consente a Internet Explorer in Windows XP, in Windows Vista o Windows Server 2008 per analizzare i messaggi di handshake TLS/SSL frammentati.

Percorso del Registro di sistema: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Per specificare una dimensione massima consentita di messaggi di handshake TLS frammentati che il client TLS accetterà, creare un **MessageLimitClient** voce. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderato. Se non è configurato, il valore predefinito sarà 0x8000 byte. 

Per specificare una dimensione massima consentita di messaggi di handshake TLS frammentati che il server TLS accetterà quando è presente alcuna autenticazione client, creare un **MessageLimitServer** voce. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderato. Se non è configurato, il valore predefinito sarà 0x4000 byte. 

Per specificare una dimensione massima consentita di messaggi di handshake TLS frammentati che il server TLS accetterà quando è presente l'autenticazione client, creare un **MessageLimitServerClientAuth** voce. Dopo aver creato la voce, modificare il valore DWORD per la lunghezza in bit desiderato. Se non è configurato, il valore predefinito sarà 0x8000 byte. 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Questa voce controlla il flag usato quando viene inviato l'elenco di autorità emittenti attendibili. Nel caso di server che considerano attendibili centinaia di autorità di certificazione per l'autenticazione client, esistono troppi delle autorità di certificazione per il server di essere in grado di inviare tutti al computer client quando si richiede l'autenticazione client. In questo caso, puoi impostare questa chiave del Registro di sistema e invece di inviare un elenco parziale, SSP Schannel non invierà alcun elenco al client.

Il mancato invio di un elenco di autorità emittenti attendibili può influire sulle quali il client invia quando viene richiesto un certificato client. Ad esempio, quando Internet Explorer riceve una richiesta di autenticazione client, Visualizza solo i certificati client concatenati a una delle autorità di certificazione che viene inviato dal server. Se il server non ha inviato un elenco, Internet Explorer visualizza tutti i certificati client installati nel client. 

Questo comportamento potrebbe risultare utile. Ad esempio, quando gli ambienti PKI includono certificati incrociati, i certificati client e server non avranno la stessa radice CA; di conseguenza, Internet Explorer non potrà scegliere un certificato concatenato a una delle CA del server. Configurazione del server che non venga inviato un elenco di autorità emittenti attendibili, Internet Explorer invierà tutti i propri certificati.

Questa voce non esiste nel Registro di sistema per impostazione predefinita.

Comportamento di invio di elenco di autorità emittenti attendibili predefinito

| Versione di Windows | Ora |
|-----------------|------|
| Windows Server 2012 e Windows 8 e versioni successive | FALSE |
| Windows Server 2008 R2 e Windows 7 e versioni precedenti | TRUE |

Versioni applicabili: come indicato nel **si applica a** elenco all'inizio di questo argomento.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>Valore di ServerCacheTime

Questa voce controlla la quantità di tempo in millisecondi, che il sistema operativo impiega per la scadenza delle voci della cache sul lato server. Il valore 0 disabilita la cache della sessione sul lato server e impedisce la riconnessione. L'aumento del valore di ServerCacheTime oltre i valori predefiniti causa Lsass.exe per l'utilizzo di memoria aggiuntiva. Ogni elemento della cache della sessione richiede in genere da 2 a 4 KB di memoria. Questa voce non esiste nel Registro di sistema per impostazione predefinita. 

Versioni applicabili: come indicato nel **si applica a** elenco all'inizio di questo argomento.

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempi di cache server predefiniti: 10 ore

## <a name="ssl-20"></a>SSL 2.0

Questa sottochiave controlla l'uso di SSL 2.0.

A partire da Windows 10 versione 1607 e Windows Server 2016, SSL 2.0 è stato rimosso e non è più supportata.
Per un impostazioni predefinite di SSL 2.0, vedere [i protocolli TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo SSL 2.0, creare un **abilitato** voce nella sottochiave appropriata. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. Per disabilitare il protocollo, modificare il valore DWORD in 0.

Tabella delle sottochiavi di SSL 2.0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di SSL 2.0 nel client SSL. |
| Server | Controlla l'uso di SSL 2.0 nel server SSL. |
| DisabledByDefault | Flag per disabilitare SSL 2.0 per impostazione predefinita. |

## <a name="ssl-30"></a>SSL 3.0

Questa sottochiave controlla l'uso di SSL 3.0.

A partire da Windows 10 versione 1607 e Windows Server 2016, SSL 3.0 è stato disattivato per impostazione predefinita. Per le impostazioni predefinite di SSL 3.0, vedere [i protocolli TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo SSL 3.0, creare una voce abilitato nella sottochiave appropriata. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. Per disabilitare il protocollo, modificare il valore DWORD in 0.

Tabella delle sottochiavi di SSL 3.0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di SSL 3.0 nel client SSL. |
| Server | Controlla l'uso di SSL 3.0 nel server SSL. |
| DisabledByDefault | Flag per disabilitare SSL 3.0 per impostazione predefinita. |

## <a name="tls-10"></a>TLS 1.0

Questa sottochiave controlla l'uso di TLS 1.0.

Per le impostazioni predefinite di TLS 1.0, vedere vedere [i protocolli TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per disabilitare il protocollo TLS 1.0, creare un **abilitato** voce nella sottochiave appropriata. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 0. Per abilitare il protocollo, modificare il valore DWORD in 1.

Tabella delle sottochiavi di TLS 1.0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di TLS 1.0 nel client TLS. |
| Server | Controlla l'uso di TLS 1.0 nel server di TLS. |
| DisabledByDefault | Flag per disabilitare TLS 1.0 per impostazione predefinita. |

## <a name="tls-11"></a>TLS 1.1

Questa sottochiave controlla l'uso di TLS 1.1.

Per le impostazioni predefinite di TLS 1.1, vedere [i protocolli TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

>[!Note] 
>Per TLS 1.1 l'abilitazione e negoziato nei server che eseguono Windows Server 2008 R2, è necessario creare il **DisabledByDefault** voce nella sottochiave appropriata (Client o Server) e impostarla su "0". Non verrà visualizzata la voce del Registro di sistema e l'impostazione predefinita viene impostata su "1".

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per disabilitare il protocollo TLS 1.1, creare un **abilitato** voce nella sottochiave appropriata. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 0. Per abilitare il protocollo, modificare il valore DWORD in 1.

Tabella delle sottochiavi di TLS 1.1

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di TLS 1.1 nel client TLS. |
| Server | Controlla l'uso di TLS 1.1 nel server di TLS. |
| DisabledByDefault | Flag per disabilitare TLS 1.1 per impostazione predefinita. |

## <a name="tls-12"></a>TLS 1.2

Questa sottochiave controlla l'uso di TLS 1.2.

Per le impostazioni predefinite di TLS 1.2, vedere [i protocolli TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

>[!Note] 
>Per l'abilitazione e negoziato nei server che eseguono Windows Server 2008 R2 in TLS 1.2, è necessario creare il **DisabledByDefault** voce nella sottochiave appropriata (Client o Server) e impostarla su "0". Non verrà visualizzata la voce del Registro di sistema e l'impostazione predefinita viene impostata su "1".

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per disabilitare il protocollo TLS 1.2, creare un **abilitato** voce nella sottochiave appropriata. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 0. Per abilitare il protocollo, modificare il valore DWORD in 1.

Tabella delle sottochiavi di TLS 1.2

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di TLS 1.2 nel client TLS. |
| Server | Controlla l'uso di TLS 1.2 nel server di TLS. |
| DisabledByDefault | Flag per disabilitare TLS 1.2 per impostazione predefinita. |

## <a name="dtls-10"></a>DTLS 1.0

Questa sottochiave controlla l'uso di DTLS 1.0.

Per le impostazioni predefinite di DTLS 1.0, vedere [i protocolli TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per disabilitare il protocollo DTLS 1.0, creare un **abilitato** voce nella sottochiave appropriata. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 0. Per abilitare il protocollo, modificare il valore DWORD in 1.

Tabella delle sottochiavi di DTLS 1.0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di DTLS 1.0 nel client DTLS. |
| Server | Controlla l'uso di DTLS 1.0 nel server di DTLS. |
| DisabledByDefault | Flag per disabilitare DTLS 1.0 per impostazione predefinita. |

## <a name="dtls-12"></a>DTLS 1.2

Questa sottochiave controlla l'uso di DTLS 1.2.

Per le impostazioni predefinite di DTLS 1.2, vedere [i protocolli TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del Registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per disabilitare il protocollo DTLS 1.2, creare un **abilitato** voce nella sottochiave appropriata. Questa voce non esiste nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 0. Per abilitare il protocollo, modificare il valore DWORD in 1.

Tabella delle sottochiavi di DTLS 1.2

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di DTLS 1.2 nel client DTLS. |
| Server | Controlla l'uso di DTLS 1.2 nel server di DTLS. |
| DisabledByDefault | Flag per disabilitare DTLS 1.2 per impostazione predefinita. |

