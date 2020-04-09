---
title: Impostazioni del registro di sistema Transport Layer Security (TLS)
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic
ms.date: 02/28/2019
ms.openlocfilehash: d65b314d6896c886ce606d2b649fcfd7309c583b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820284"
---
# <a name="transport-layer-security-tls-registry-settings"></a>Impostazioni del registro di sistema Transport Layer Security (TLS)

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10

Questo argomento di riferimento per i professionisti IT contiene informazioni sulle impostazioni del registro di sistema supportate per l'implementazione di Windows del protocollo Transport Layer Security (TLS) e il protocollo di Secure Sockets Layer (SSL) tramite il provider di supporto della sicurezza (SSP) Schannel. Le sottochiavi e le voci del registro di sistema descritte in questo argomento consentono di amministrare e risolvere i problemi relativi a SSP Schannel, in particolare i protocolli TLS e SSL. 

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

Versioni applicabili: come indicato nell'elenco **si applica a** all'inizio di questo argomento.

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Ciphers

La crittografia TLS/SSL deve essere controllata tramite la configurazione dell'ordine del pacchetto di crittografia. Per informazioni dettagliate, vedere [configurazione dell'ordine del pacchetto di crittografia TLS](manage-tls.md#configuring-tls-cipher-suite-order).

Per informazioni sugli ordini predefiniti dei pacchetti di crittografia usati da SSP Schannel, vedere pacchetti [di crittografia in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 

## <a name="ciphersuites"></a>CipherSuites

Per configurare i pacchetti di crittografia TLS/SSL, è necessario usare criteri di gruppo, MDM o PowerShell. per informazioni dettagliate, vedere Configurazione di un [pacchetto di crittografia TLS](manage-tls.md#configuring-tls-cipher-suite-order) .

Per informazioni sugli ordini predefiniti dei pacchetti di crittografia usati da SSP Schannel, vedere pacchetti [di crittografia in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 


## <a name="clientcachetime"></a>ClientCacheTime

Questa voce controlla l'intervallo di tempo, in millisecondi, che il sistema operativo impiega per la scadenza delle voci della cache sul lato client. Il valore 0 disattiva la memorizzazione nella cache per la connessione sicura. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. 

La prima volta che un client si connette a un server tramite SSP Schannel, viene eseguito un handshake TLS/SSL completo. Al termine, master secret, pacchetto di crittografia e certificati vengono archiviati nella cache della sessione sui rispettivi client e server.

A partire da Windows Server 2008 e Windows Vista, il tempo di memorizzazione nella cache client predefinito è 10 ore.

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempi di memorizzazione nella cache client predefiniti

## <a name="enableocspstaplingforsni"></a>EnableOcspStaplingForSni

Il protocollo di stato del certificato online (OCSP) consente a un server Web, ad esempio Internet Information Services (IIS), di fornire lo stato di revoca corrente di un certificato del server quando invia il certificato del server a un client durante l'handshake TLS. Questa funzionalità riduce il carico sui server OCSP perché il server Web è in grado di memorizzare nella cache lo stato OCSP corrente del certificato del server e di inviarlo a più client Web. Senza questa funzionalità, ogni client Web tenterà di recuperare lo stato OCSP corrente del certificato server dal server OCSP. Questo genererebbe un carico elevato sul server OCSP. 

Oltre a IIS, i servizi Web tramite http. sys possono trarre vantaggio anche da questa impostazione, tra cui Active Directory Federation Services (AD FS) e proxy applicazione Web (WAP). 

Per impostazione predefinita, il supporto OCSP è abilitato per i siti Web IIS che dispongono di un binding sicuro (SSL/TLS) semplice. Tuttavia, questo supporto non è abilitato per impostazione predefinita se il sito Web IIS usa uno o entrambi i tipi di binding Secure (SSL/TLS) seguenti:
- Richiedi Indicazione nome server
- Usa archivio certificati centralizzato

In questo caso, la risposta Hello del server durante l'handshake TLS non includerà uno stato graffettato OCSP per impostazione predefinita. Questo comportamento migliora le prestazioni: l'implementazione di graffatura di Windows OCSP è scalabile fino a centinaia di certificati del server. Poiché SNI e CCS consentono a IIS di passare a migliaia di siti Web che potenzialmente hanno migliaia di certificati del server, l'impostazione di questo comportamento come abilitata per impostazione predefinita può causare problemi di prestazioni.

Versioni applicabili: tutte le versioni che iniziano con Windows Server 2012 e Windows 8. 

Percorso del registro di sistema: [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL]

Aggiungere la chiave seguente:

"EnableOcspStaplingForSni" = DWORD: 00000001

Per disabilitare, impostare il valore DWORD su 0:

"EnableOcspStaplingForSni" = DWORD: 00000000

> [!NOTE] 
> L'abilitazione di questa chiave del registro di sistema ha un potenziale impatto sulle prestazioni.

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Questa voce controlla la conformità con FIPS (Federal Information Processing Standard). Il valore predefinito è 0.

Versioni applicabili: tutte le versioni che iniziano con Windows Server 2012 e Windows 8. 

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\LSA

Pacchetti di crittografia FIPS per Windows Server: vedere pacchetti [di crittografia e protocolli supportati in SSP Schannel](https://technet.microsoft.com/library/dn786419.aspx).

## <a name="hashes"></a>Hashes

È necessario controllare gli algoritmi hash TLS/SSL configurando l'ordine del pacchetto di crittografia. Per informazioni dettagliate, vedere [configurazione dell'ordine del pacchetto di crittografia TLS](manage-tls.md#configuring-tls-cipher-suite-order) .

## <a name="issuercachesize"></a>IssuerCacheSize

Questa voce controlla le dimensioni della cache dell'autorità di certificazione ed è usata con il mapping dell'autorità di certificazione. SSP Schannel tenta di eseguire il mapping di tutte le autorità di certificazione nella catena di certificati del client, non solo l'emittente diretta del certificato client. Quando non viene eseguito il mapping delle autorità di certificazione a un account, un caso tipico, il server potrebbe provare a eseguire ripetutamente il mapping dello stesso nome dell'autorità di certificazione, centinaia di volte al secondo. 

Per evitare questo problema, nel server è disponibile una cache negativa, quindi se il mapping di un nome dell'autorità di certificazione a un account non viene eseguito, viene aggiunto alla cache e SSP Schannel non proverà a eseguire di nuovo il mapping di quel nome finché la voce della cache non scadrà. Questa voce del Registro di sistema specifica le dimensioni della cache. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Il valore predefinito è 100. 

Versioni applicabili: tutte le versioni che iniziano con Windows Server 2008 e Windows Vista.

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Questa voce controlla la durata dell'intervallo di timeout della cache in millisecondi. SSP Schannel tenta di eseguire il mapping di tutte le autorità di certificazione nella catena di certificati del client, non solo l'emittente diretta del certificato client. Se non viene eseguito il mapping delle autorità di certificazione a un account, un caso tipico, il server potrebbe provare a eseguire ripetutamente il mapping dello stesso nome dell'autorità di certificazione, centinaia di volte al secondo.

Per evitare questo problema, nel server è disponibile una cache negativa, quindi se il mapping di un nome dell'autorità di certificazione a un account non viene eseguito, viene aggiunto alla cache e SSP Schannel non proverà a eseguire di nuovo il mapping di quel nome finché la voce della cache non scadrà. Questa cache viene mantenuta ai fini delle prestazioni, per evitare che il sistema continui a provare a eseguire il mapping delle stesse autorità di certificazione. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Il valore predefinito è 10 minuti.

Versioni applicabili: tutte le versioni che iniziano con Windows Server 2008 e Windows Vista.

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm-dimensioni delle chiavi RSA del client

Questa voce Controlla le dimensioni della chiave RSA del client. 

Per controllare l'utilizzo degli algoritmi di scambio delle chiavi, è necessario configurare l'ordine del pacchetto di crittografia.

Aggiunta in Windows 10, versione 1507 e Windows Server 2016.

Percorso del registro di sistema: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Per specificare un intervallo minimo supportato di lunghezza in bit della chiave RSA per il client TLS, creare una voce **ClientMinKeyBitLength** . Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD sulla lunghezza in bit desiderata. Se non è configurato, 1024 bit sarà il minimo. 

Per specificare un intervallo massimo supportato di lunghezza in bit della chiave RSA per il client TLS, creare una voce **ClientMaxKeyBitLength** . Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD sulla lunghezza in bit desiderata. Se non è configurato, non viene applicato un valore massimo.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm-dimensioni della chiave Diffie-Hellman

Questa voce Controlla le dimensioni della chiave Diffie-Hellman. 

Per controllare l'utilizzo degli algoritmi di scambio delle chiavi, è necessario configurare l'ordine del pacchetto di crittografia.

Aggiunta in Windows 10, versione 1507 e Windows Server 2016.

Percorso del registro di sistema: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Per specificare un intervallo minimo supportato per la lunghezza in bit della chiave Diffie-Helman per il client TLS, creare una voce **ClientMinKeyBitLength** . Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD sulla lunghezza in bit desiderata. Se non è configurato, 1024 bit sarà il minimo. 
 
Per specificare un intervallo massimo supportato per la lunghezza in bit della chiave Diffie-Helman per il client TLS, creare una voce **ClientMaxKeyBitLength** . Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD sulla lunghezza in bit desiderata. Se non è configurato, non viene applicato un valore massimo. 
 
Per specificare la lunghezza in bit della chiave Diffie-Helman per l'impostazione predefinita del server TLS, creare una voce **ServerMinKeyBitLength** . Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD sulla lunghezza in bit desiderata. Se non è configurato, 2048 bit sarà il valore predefinito. 

## <a name="maximumcachesize"></a>MaximumCacheSize

Questa voce controlla il numero massimo di elementi della cache. L'impostazione di Setting MaximumCacheSize su 0 disabilita la cache della sessione sul lato server e impedisce la riconnessione. L'aumento del valore di MaximumCacheSize oltre i valori predefiniti causa il consumo di memoria aggiuntiva da parte di Lsass.exe. Ogni elemento della cache della sessione richiede in genere da 2 a 4 KB di memoria. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Il valore predefinito è 20.000 elementi. 

Versioni applicabili: tutte le versioni che iniziano con Windows Server 2008 e Windows Vista.

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Messaggistica-analisi dei frammenti

________________________________________
Questa voce Controlla le dimensioni massime consentite dei messaggi di handshake TLS frammentati che verranno accettati. I messaggi di dimensioni superiori a quelle consentite non verranno accettati e l'handshake TLS avrà esito negativo. Per impostazione predefinita, queste voci non sono presenti nel registro di sistema. 

Quando si imposta il valore su 0x0, i messaggi frammentati non vengono elaborati e l'handshake TLS avrà esito negativo. In questo modo i client o i server TLS nel computer corrente non sono conformi alle RFC TLS. 

Le dimensioni massime consentite possono essere aumentate fino a 2 ^ 24-1 byte. Per consentire a un client o a un server di leggere e archiviare grandi quantità di dati non verificati dalla rete non è una buona idea e utilizzerà memoria aggiuntiva per ogni contesto di sicurezza. 

Aggiunta in Windows 7 e Windows Server 2008 R2.
È disponibile un aggiornamento che consente a Internet Explorer in Windows XP, in Windows Vista o in Windows Server 2008 di analizzare i messaggi di handshake TLS/SSL frammentati.

Percorso del registro di sistema: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Per specificare una dimensione massima consentita dei messaggi di handshake TLS frammentati che il client TLS accetterà, creare una voce **MessageLimitClient** . Dopo aver creato la voce, modificare il valore DWORD sulla lunghezza in bit desiderata. Se non è configurato, il valore predefinito sarà 0x8000 bytes. 

Per specificare una dimensione massima consentita dei messaggi di handshake TLS frammentati che il server TLS accetterà quando non è disponibile l'autenticazione client, creare una voce **MessageLimitServer** . Dopo aver creato la voce, modificare il valore DWORD sulla lunghezza in bit desiderata. Se non è configurato, il valore predefinito sarà 0x4000 bytes. 

Per specificare una dimensione massima consentita dei messaggi di handshake TLS frammentati che il server TLS accetterà quando è presente l'autenticazione client, creare una voce **MessageLimitServerClientAuth** . Dopo aver creato la voce, modificare il valore DWORD sulla lunghezza in bit desiderata. Se non è configurato, il valore predefinito sarà 0x8000 bytes. 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Questa voce controlla il flag usato quando viene inviato l'elenco delle autorità di certificazione attendibili. Nel caso di server che considerano attendibili centinaia di autorità di certificazione per l'autenticazione client, il server non potrà inviare tutte queste autorità di certificazione al computer client quando richiede l'autorizzazione client a causa del loro numero eccessivo. In questa situazione, è possibile impostare questa chiave del Registro di sistema e, invece di inviare un elenco parziale, SSP Schannel non invierà alcun elenco al client.

Il mancato invio di un elenco di autorità di certificazione attendibili potrebbe avere un impatto su ciò che il client invia quando viene richiesto un certificato client. Ad esempio, quando Internet Explorer riceve una richiesta di autenticazione client, visualizza solo i certificati client concatenati a una delle autorità di certificazione inviata dal server. Se il server non ha inviato un elenco, Internet Explorer visualizza tutti i certificati client installati nel client. 

Questo comportamento potrebbe risultare utile. Ad esempio, quando gli ambienti PKI includono certificati incrociati, i certificati client e server non avranno la stessa CA radice. Pertanto, Internet Explorer non può scegliere un certificato concatenato a una delle CA del server. Se si configura il server in modo che non invii un elenco di autorità di certificazione attendibili, Internet Explorer invierà tutti i propri certificati.

Questa voce non è disponibile nel Registro di sistema per impostazione predefinita.

Comportamento predefinito di invio dell'elenco di autorità emittenti attendibili

| Versione di Windows | Tempo |
|-----------------|------|
| Windows Server 2012 e Windows 8 e versioni successive | FALSE |
| Windows Server 2008 R2 e Windows 7 e versioni precedenti | TRUE |

Versioni applicabili: tutte le versioni che iniziano con Windows Server 2008 e Windows Vista.

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

Questa voce controlla l'intervallo di tempo, in millisecondi, che il sistema operativo impiega per la scadenza delle voci della cache sul lato server. Un valore 0 disabilita la cache della sessione sul lato server e impedisce la riconnessione. L'aumento del valore di ServerCacheTime oltre i valori predefiniti causa il consumo di memoria aggiuntiva da parte di Lsass.exe. Ogni elemento della cache della sessione richiede in genere da 2 a 4 KB di memoria. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. 

Versioni applicabili: tutte le versioni che iniziano con Windows Server 2008 e Windows Vista.

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Tempo predefinito per la cache del server: 10 ore

## <a name="ssl-20"></a>SSL 2.0

Questa sottochiave controlla l'uso di SSL 2,0.

A partire da Windows 10, versione 1607 e Windows Server 2016, SSL 2,0 è stato rimosso e non è più supportato.
Per le impostazioni predefinite di SSL 2,0, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo SSL 2,0, creare una voce **Enabled** nella sottochiave client o server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi SSL 2,0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di SSL 2,0 sul client SSL. |
| Server | Controlla l'uso di SSL 2,0 sul server SSL. |

Per disabilitare SSL 2,0 per il client o il server, modificare il valore DWORD in 0. Se un'app SSPI richiede l'uso di SSL 2,0, verrà negata. 

Per disabilitare SSL 2,0 per impostazione predefinita, creare una voce **DisabledByDefault** e modificare il valore DWORD in 1. Se un'app SSPI esplicitamente richiede l'uso di SSL 2,0, può essere negoziata. 

Nell'esempio seguente viene mostrato SSL 2,0 disabilitato nel registro di sistema:

![SSL 2,0 disabilitato](images/ssl-2-registry-setting.png)


## <a name="ssl-30"></a>SSL 3.0

Questa sottochiave controlla l'uso di SSL 3,0.

A partire da Windows 10, versione 1607 e Windows Server 2016, SSL 3,0 è stato disabilitato per impostazione predefinita. Per le impostazioni predefinite di SSL 3,0, vedere [protocolli in TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo SSL 3,0, creare una voce **Enabled** nella sottochiave client o server, come descritto nella tabella seguente.  
Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi SSL 3,0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di SSL 3,0 sul client SSL. |
| Server | Controlla l'uso di SSL 3,0 sul server SSL. |

Per disabilitare SSL 3,0 per il client o il server, modificare il valore DWORD in 0.
Se un'app SSPI richiede l'uso di SSL 3,0, verrà negata. 

Per disabilitare SSL 3,0 per impostazione predefinita, creare una voce **DisabledByDefault** e modificare il valore DWORD in 1. Se un'app SSPI richiede in modo esplicito l'uso di SSL 3,0, può essere negoziata. 

Nell'esempio seguente viene mostrato SSL 3,0 disabilitato nel registro di sistema:

![SSL 3,0 disabilitato](images/ssl-3-registry-setting.png)

## <a name="tls-10"></a>TLS 1.0

Questa sottochiave controlla l'uso di TLS 1,0.

Per le impostazioni predefinite di TLS 1,0, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo TLS 1,0, creare una voce **Enabled** nella sottochiave client o server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi di TLS 1,0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di TLS 1,0 sul client TLS. |
| Server | Controlla l'uso di TLS 1,0 sul server TLS. |

Per disabilitare TLS 1,0 per il client o il server, modificare il valore DWORD in 0.
Se un'app SSPI richiede l'uso di TLS 1,0, verrà negata. 

Per disabilitare TLS 1,0 per impostazione predefinita, creare una voce **DisabledByDefault** e modificare il valore DWORD in 1. Se un'app SSPI richiede in modo esplicito l'uso di TLS 1,0, può essere negoziata. 

Nell'esempio seguente viene illustrato TLS 1,0 disabilitato nel registro di sistema:

![TLS 1,0 disabilitato](images/tls-registry-setting.png)

## <a name="tls-11"></a>TLS 1.1

Questa sottochiave controlla l'uso di TLS 1,1.

Per le impostazioni predefinite di TLS 1,1, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo TLS 1,1, creare una voce **Enabled** nella sottochiave client o server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi di TLS 1,1

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di TLS 1,1 sul client TLS. |
| Server | Controlla l'uso di TLS 1,1 sul server TLS. |

Per disabilitare TLS 1,1 per il client o il server, modificare il valore DWORD in 0.
Se un'app SSPI richiede l'uso di TLS 1,1, verrà negata. 

Per disabilitare TLS 1,1 per impostazione predefinita, creare una voce **DisabledByDefault** e modificare il valore DWORD in 1. Se un'app SSPI richiede in modo esplicito l'uso di TLS 1,1, può essere negoziata. 

Nell'esempio seguente viene illustrato TLS 1,1 disabilitato nel registro di sistema:

![TLS 1,1 disabilitato](images/tls-11-registry-setting.png)

## <a name="tls-12"></a>TLS 1.2

Questa sottochiave controlla l'uso di TLS 1,2.

Per le impostazioni predefinite di TLS 1,2, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo TLS 1,2, creare una voce **Enabled** nella sottochiave client o server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi di TLS 1,2

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di TLS 1,2 sul client TLS. |
| Server | Controlla l'uso di TLS 1,2 sul server TLS. |

Per disabilitare TLS 1,2 per il client o il server, modificare il valore DWORD in 0.
Se un'app SSPI richiede l'uso di TLS 1,2, verrà negata. 

Per disabilitare TLS 1,2 per impostazione predefinita, creare una voce **DisabledByDefault** e modificare il valore DWORD in 1. Se un'app SSPI richiede in modo esplicito l'uso di TLS 1,2, può essere negoziata. 

Nell'esempio seguente viene illustrato TLS 1,2 disabilitato nel registro di sistema:

![TLS 1,2 disabilitato](images/tls-12-registry-setting.png)

## <a name="dtls-10"></a>DTLS 1.0

Questa sottochiave controlla l'uso di DTLS 1,0.

Per le impostazioni predefinite di DTLS 1,0, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo DTLS 1,0, creare una voce **Enabled** nella sottochiave client o server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi DTLS 1,0

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di DTLS 1,0 nel client DTLS. |
| Server | Controlla l'uso di DTLS 1,0 nel server DTLS. |

Per disabilitare DTLS 1,0 per il client o il server, modificare il valore DWORD in 0.
Se un'app SSPI richiede l'uso di DTLS 1,0, verrà negata. 

Per disabilitare DTLS 1,0 per impostazione predefinita, creare una voce **DisabledByDefault** e modificare il valore DWORD in 1. Se un'app SSPI richiede in modo esplicito l'uso di DTLS 1,0, può essere negoziata. 

L'esempio seguente mostra DTLS 1,0 disabilitato nel registro di sistema:

![DTLS 1,0 disabilitato](images/dtls-10-registry-setting.png)

## <a name="dtls-12"></a>DTLS 1,2

Questa sottochiave controlla l'uso di DTLS 1,2.

Per le impostazioni predefinite di DTLS 1,2, vedere [protocolli in TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Percorso del registro di sistema: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Per abilitare il protocollo DTLS 1,2, creare una voce **Enabled** nella sottochiave client o server, come descritto nella tabella seguente. Questa voce non è disponibile nel Registro di sistema per impostazione predefinita. Dopo aver creato la voce, modificare il valore DWORD in 1. 

Tabella delle sottochiavi DTLS 1,2

| Sottochiave | Descrizione |
|--------|-------------|
| Client | Controlla l'uso di DTLS 1,2 nel client DTLS. |
| Server | Controlla l'uso di DTLS 1,2 nel server DTLS. |


Per disabilitare DTLS 1,2 per il client o il server, modificare il valore DWORD in 0.
Se un'app SSPI richiede l'uso di DTLS 1,0, verrà negata. 

Per disabilitare DTLS 1,2 per impostazione predefinita, creare una voce **DisabledByDefault** e modificare il valore DWORD in 1. Se un'app SSPI richiede in modo esplicito l'uso di DTLS 1,2, può essere negoziata. 

L'esempio seguente mostra DTLS 1,1 disabilitato nel registro di sistema:

![DTLS 1,1 disabilitato](images/dtls-11-registry-setting.png)


