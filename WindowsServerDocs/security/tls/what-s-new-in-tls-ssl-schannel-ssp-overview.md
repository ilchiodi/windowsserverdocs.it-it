---
title: TLS / SSL (Schannel SSP) Panoramica
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: b1af556200c9dd497bac835f1480479cca075dab
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447280"
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>Panoramica di TLS / SSL (SSP Schannel)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

In questo argomento destinato ai professionisti IT descrive le modifiche delle funzionalità in Schannel Security Support Provider (SSP), che include la sicurezza TLS (Transport Layer), il Secure Sockets Layer (SSL) e Datagram Transport Layer Security (DTLS) protocolli di autenticazione per Windows Server 2012 R2, Windows Server 2012, Windows 8.1 e Windows 8.

Schannel è un Security Support Provider (SSP) che implementa i protocolli di autenticazione standard Internet SSL, TLS E DTLS. Security Support Provider Interface (SSPI) è un'API utilizzata dai sistemi Windows per eseguire funzioni correlate alla sicurezza, tra cui l'autenticazione. SSPI funziona come un'interfaccia comune per diversi Security Support Provider (SSP), tra cui l'SSP Schannel.

Per altre informazioni sull'implementazione Microsoft di TLS e SSL nel SSP Schannel, vedere la [tecnica su TLS/SSL (2003)](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx).


## <a name="tlsssl-schannel-ssp-features"></a>Funzionalità TLS/SSL (SSP Schannel)
Di seguito vengono descritte le funzionalità di TLS in SSP. Schannel.

### <a name="tls-session-resumption"></a>Ripresa della sessione TLS
Il protocollo Transport Layer Security (TLS), un componente del Security Support Provider Schannel, viene usato per proteggere i dati inviati tra le applicazioni in una rete non attendibile. TLS/SSL può essere usato per autenticare server e computer client e anche per crittografare i messaggi tra le entità autenticate.

I dispositivi che connettono spesso TLS ai server devono riconnettersi a causa della scadenza della sessione. Windows 8.1 e Windows Server 2012 R2 ora supportano RFC 5077 (ripresa della sessione TLS senza stato sul lato Server). Questa modifica fornisce i dispositivi Windows Phone e Windows RT con:

-   Riduzione dell'utilizzo di risorse nel server.

-   Riduzione della larghezza di banda, che determina un miglioramento dell'efficienza delle connessioni client.

-   Riduzione del tempo impiegato per l'handshake TLS a causa delle riprese della connessione.

> [!NOTE]
> L'implementazione lato client di RFC 5077 è stata aggiunta in Windows 8.

Per informazioni sulla ripresa della sessione TLS senza stato, vedere il documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

### <a name="application-protocol-negotiation"></a>Negoziazione del protocollo applicativo
 Windows Server 2012 R2 e Windows 8.1 supportano negoziazione del protocollo applicativo TLS sul lato client, in modo che le applicazioni possono sfruttare i protocolli come parte dello sviluppo standard HTTP 2.0 e gli utenti possono accedere a servizi online come Google e Twitter usando le App in esecuzione il protocollo SPDY.

**Come funziona**

Le applicazioni client e server abilitano l'estensione della negoziazione del protocollo applicativo fornendo elenchi di identificatori di protocolli applicativi supportati, in ordine di preferenza decrescente. Il client TLS indica il supporto della negoziazione del protocollo applicativo includendo l'estensione Application Layer Protocol Negotiation (ALPN) con un elenco di protocolli supportati dal client nel messaggio ClientHello.

Quando il client TLS effettua la richiesta al server, il server TLS legge l'elenco dei protocolli supportati per cercare il protocollo applicativo preferito, anch'esso supportato dal client. Se il protocollo viene trovato, il server risponde con l'identificatore di protocollo selezionato e continua con l'handshake come di consueto. Se non è presente alcun protocollo applicativo comune, il server invia un avviso di errore handshake irreversibile.

### <a name="BKMK_TrustedIssuers"></a>Gestione di autorità emittenti attendibili per l'autenticazione client
Quando è necessario effettuare l'autenticazione del computer client con SSL o TLS, il server può essere configurato per l'invio di un elenco di autorità di certificazione attendibili. L'elenco contiene un set di autorità di certificazione che il server considererà attendibili e propone al computer client il certificato client da selezionare se sono presenti più certificati. Inoltre, la catena di certificati che il computer client invia al server deve essere convalidata rispetto all'elenco di autorità emittenti attendibili configurate.

Prima di Windows Server 2012 e Windows 8, le applicazioni o i processi che SSP Schannel (inclusi HTTP. sys e IIS) potevano fornire un elenco di autorità emittenti attendibili supportate per l'autenticazione Client tramite un elenco dei certificati attendibili (CTL).

In Windows Server 2012 e Windows 8, sono state apportate modifiche al processo di autenticazione sottostante in modo che:

-   La gestione dell'elenco di autorità emittenti attendibili basata sull'elenco scopi consentiti non sia più supportata.

-   Il comportamento di invio dell'elenco di autorità emittenti attendibili sia disattivato per impostazione predefinita. Il valore predefinito della chiave del registro di sistema SendTrustedIssuerList è ora 0 (disattivato per impostazione predefinita) anziché 1.

-   Venga mantenuta la compatibilità con le versioni precedenti dei sistemi operativi Windows.

**Valore questo aggiunto?**

A partire da Windows Server 2012, l'uso di CTL è stato sostituito con un'implementazione basata su archivio certificati. Questo consente una maggiore familiarità della gestibilità tramite i cmdlet esistenti per la gestione dei certificati del provider PowerShell, nonché gli strumenti da riga di comando come certutil.exe.

Anche se le dimensioni massime di nell'elenco di autorità di certificazione attendibili supportate da parte che SSP Schannel (16 KB) resta analogo a quello in Windows Server 2008 R2, Windows Server 2012 prevede un nuovo archivio certificati dedicato per emittenti per l'autenticazione client, in modo che i certificati non correlati non sono inclusi nel messaggio.

**Come funziona?**

In Windows Server 2012, l'elenco di autorità emittenti attendibili viene configurato usando gli archivi certificati; archivio certificati del computer globale uno predefinito e uno che è facoltativo per ogni sito. L'origine dell'elenco verrà determinata come segue:

-   Se è presente un archivio credenziali specifico configurato per il sito, verrà usato come origine.

-   Se non esistono certificati nell'archivio definito dall'applicazione, Schannel controlla l'archivio di **autorità emittenti per l'autenticazione client** del computer locale e, se sono presenti certificati, usa tale archivio come origine. Se non vengono trovati certificati in nessuno dei due archivi, viene controllato l'archivio delle radici attendibili.

-   Se nessuno dei due archivi locale o globale contengono certificati, il provider Schannel userà il **delle radici attendibili** archiviare come l'origine dell'elenco di autorità emittenti attendibili. (Si tratta del comportamento per Windows Server 2008 R2).

Se il **delle radici attendibili** store usato contiene una combinazione di radice (autofirmati) e i certificati delle autorità di certificazione dell'autorità (CA), solo i certificati di autorità di certificazione dell'autorità di certificazione verranno inviati al server per impostazione predefinita.

**Come configurare Schannel per usare l'archivio certificati delle autorità emittenti attendibili**

L'architettura di SSP Schannel in Windows Server 2012 per impostazione predefinita utilizzerà l'Archivia come descritto in precedenza per gestire l'elenco di autorità emittenti attendibili. È comunque sempre possibile usare i cmdlet esistenti per la gestione dei certificati del provider PowerShell, nonché gli strumenti da riga di comando come certutil.exe.

Per informazioni sulla gestione dei certificati con il provider PowerShell, vedere [Cmdlet di amministrazione di Servizi certificati Active Directory in Windows](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx).

Per informazioni sulla gestione dei certificati con l'utilità per i certificati, vedere [certutil.exe](https://technet.microsoft.com/library/cc732443.aspx).

Per informazioni su quali dati, tra cui l'archivio definito dall'applicazione, vengono definiti per una credenziale Schannel, vedere [Struttura SCHANNEL_CRED (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx).

**Valori predefiniti per le modalità di attendibilità**

Esistono tre modalità di attendibilità dell'autenticazione client supportate dal provider Schannel. La modalità di attendibilità controlla come viene eseguita la convalida della catena di certificati del client e un'impostazione a livello di sistema controllata da REG_DWORD "ClientAuthTrustMode" in hkey_local_machine\system\currentcontrolset\control\securityproviders\schannel. .

|Value|Modalità di attendibilità|Descrizione|
|-----|-------|--------|
|0|Machine Trust (impostazione predefinita)|Richiede che il certificato client venga emesso da un'autorità di certificazione inclusa nell'elenco di autorità emittenti attendibili.|
|1|Exclusive Root Trust|Richiede che un certificato client sia concatenato a un certificato radice contenuto nell'archivio di autorità emittenti attendibili specificato dal chiamante. Il certificato deve anche essere emesso da un'autorità emittente inclusa nell'elenco di autorità emittenti attendibili.|
|2|Exclusive CA Trust|Richiede che un certificato client sia concatenato a un certificato della CA intermedia o a un certificato radice contenuto nell'archivio di autorità emittenti attendibili specificato dal chiamante.|

Per informazioni sugli errori di autenticazione causati da problemi di configurazione delle autorità emittenti attendibili, vedere l'articolo della Knowledge Base [280256](https://support.microsoft.com/kb/2802568).

### <a name="BKMK_SNI"></a>Supporto TLS per estensioni Server Name Indicator (SNI)
La funzionalità di indicazione nome server estende i protocolli SSL e TLS per consentire la corretta identificazione del server durante l'esecuzione di numerose immagini virtuali su un singolo server. Per proteggere la comunicazione tra un computer client e un server, il computer client richiede un certificato digitale al server. Dopo che il server risponde alla richiesta e invia il certificato, il computer client lo esamina, lo usa per crittografare le comunicazioni e procede con il normale scambio di richiesta-risposta. In uno scenario di hosting virtuale, tuttavia, diversi domini, ognuno con il proprio certificato potenzialmente distinto, sono ospitati in un solo server. In questo caso, il server non ha modo di sapere in anticipo quale certificato inviare al computer client. SNI consente al computer client di informare in anticipo il dominio di destinazione nel protocollo e questo consente al server di selezionare correttamente il certificato appropriato.

**Valore questo aggiunto?**

Questa funzionalità aggiuntiva:

-   Consente di ospitare più siti Web SSL in una singola combinazione di indirizzo IP e porta.

-   Riduce l'utilizzo di memoria quando più siti Web SSL sono ospitati in un singolo server Web.

-   Consente a più utenti di connettersi ai siti Web SSL contemporaneamente.

-   Consente di fornire suggerimenti agli utenti finali tramite l'interfaccia del computer per la selezione del certificato corretto durante un processo di autenticazione client.

**Come funziona**

L'SSP Schannel mantiene una cache in memoria degli stati di connessione client consentiti per i client. In questo modo i computer client possono riconnettersi al server SSL senza dover eseguire un handshake SSL completo nelle visite successive.  Questo uso efficiente della gestione dei certificati consente a più siti da ospitare in un unico rispetto alle versioni precedenti del sistema operativo di Windows Server 2012.

La selezione dei certificati da parte dell'utente finale è stata migliorata perché consente di creare un elenco di nomi di probabili autorità di certificazione che forniscono all'utente finale suggerimenti su quale autorità scegliere. L'elenco è configurabile con Criteri di gruppo.

### <a name="BKMK_DTLS"></a>Datagram Transport Layer Security (DTLS)
Il protocollo DTLS versione 1.0 è stato aggiunto al Security Support Provider Schannel. Il protocollo DTLS fornisce la riservatezza delle comunicazioni per i protocolli di datagramma. Il protocollo consente alle applicazioni client/server di comunicare in un modo progettato per impedire l'intercettazione, la manomissione o la contraffazione dei messaggi. Il protocollo DTLS è basato sul protocollo Transport Layer Security (TLS) e fornisce garanzie di protezione equivalente, riducendo la necessità di usare IPsec o di progettare un protocollo di protezione a livello dell'applicazione personalizzata.

**Valore questo aggiunto?**

I datagrammi sono comuni nei flussi multimediali, ad esempio protetta o i giochi le videoconferenze. L'aggiunta del protocollo DTLS al provider Schannel offre la possibilità di usare il noto modello Windows SSPI per proteggere la comunicazione tra computer client e server. DTLS è deliberatamente progettato per essere il più possibile simile a TLS, sia per ridurre al minimo la creazione di nuove forme di protezione sia per ottimizzare la quantità di riutilizzo del codice e dell'infrastruttura.

**Come funziona**

Le applicazioni che usano DTLS su UDP possono usare il modello SSPI in Windows Server 2012 e Windows 8. Analogamente a TLS, alcuni pacchetti di crittografia sono disponibili per la configurazione. Schannel continua a usare il provider di crittografia di CNG che sfrutta i vantaggi della certificazione FIPS 140, introdotta in Windows Vista.

### <a name="BKMK_Deprecated"></a>Funzionalità deprecate
In SSP Schannel per Windows Server 2012 e Windows 8, non esistono alcuna funzionalità o caratteristiche deprecate.

## <a name="see-also"></a>Vedere anche
-   [Modello di sicurezza del Cloud privato: funzionalità Wrapper](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)



