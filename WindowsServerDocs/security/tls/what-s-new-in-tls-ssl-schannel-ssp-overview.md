---
title: TLS / SSL (SSP Schannel) Panoramica
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c8836345-16bb-4dcc-8d2b-2b9b687456a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: b846ed54a1f7c8ef7a85ea9f836ffa75d0036ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="overview-of-tls---ssl-schannel-ssp"></a>Panoramica di TLS / SSL (SSP Schannel)

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento, destinato ai professionisti IT descrive le modifiche delle funzionalità in Schannel Security Support Provider (SSP), che include i protocolli di autenticazione Datagram Transport Layer Security (DTLS), la sicurezza TLS (Transport Layer) e il Secure Sockets Layer (SSL) per Windows Server 2012 R2, Windows Server 2012, Windows 8.1 e Windows 8.

Schannel è un Security Support Provider (SSP) che implementa i protocolli di autenticazione standard SSL, TLS e DTLS Internet. Security Support Provider Interface (SSPI) è un'API utilizzata dai sistemi Windows per eseguire funzioni correlate alla sicurezza tra cui l'autenticazione. SSPI funziona come un'interfaccia comune per diversi Security Support Provider (SSP), tra cui SSP. Schannel.

For more information about Microsoft's implementation of TLS and SSL in the Schannel SSP, see the [TLS/SSL Technical Reference (2003)](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx).


##<a name="tlsssl-schannel-ssp-features"></a>Funzionalità TLS/SSL (SSP Schannel)
Di seguito vengono descritte le funzionalità di TLS in SSP. Schannel.

### <a name="tls-session-resumption"></a>Ripresa della sessione TLS
Il protocollo Transport Layer Security (TLS), un componente del Provider supporto sicurezza Schannel, viene utilizzato per proteggere i dati inviati tra le applicazioni su una rete non attendibile. TLS/SSL può essere usato per autenticare i server e i computer client, nonché per crittografare i messaggi tra le entità autenticate.

I dispositivi che connettono spesso TLS ai server devono riconnettersi a causa della scadenza della sessione. Windows 8.1 e Windows Server 2012 R2 ora supportano RFC 5077 (ripresa della sessione TLS senza stato sul lato Server). Questa modifica fornisce ai dispositivi Windows Phone e Windows RT con:

-   Utilizzo delle risorse ridotto nel server di

-   Riduzione della larghezza di banda, migliorando l'efficienza delle connessioni client

-   Riduzione del tempo impiegato per l'handshake TLS a causa delle riprese della connessione.

> [!NOTE]
> L'implementazione sul lato client di RFC 5077 è stata aggiunta in Windows 8.

Per informazioni sulla ripresa di sessione TLS senza stato, vedere il documento IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

### <a name="application-protocol-negotiation"></a>Negoziazione del protocollo applicativo
 Windows Server 2012 R2 e Windows 8.1 supportano negoziazione del protocollo applicativo TLS sul lato client in modo che le applicazioni possono sfruttare i protocolli come parte dello sviluppo standard HTTP 2.0 e gli utenti possono accedere a servizi online come Google e Twitter usando App che eseguono il protocollo SPDY.

**Come funziona**

Applicazioni client e server abilitare l'estensione di negoziazione del protocollo dell'applicazione fornendo elenchi di protocolli applicativi supportati ID, in ordine di preferenza decrescente. Il client TLS indica il supporto della negoziazione del protocollo applicativo includendo l'estensione Application Layer Protocol Negotiation (ALPN) con un elenco di protocolli supportati client nel messaggio ClientHello.

Quando il client TLS effettua la richiesta al server, il server TLS legge l'elenco dei protocolli supportati per il protocollo applicativo preferito che il client supporta anche. Se il protocollo viene trovato, il server risponde con l'ID di protocollo selezionato e continua con l'handshake come di consueto. Se è presente alcun protocollo applicativo comune, il server invia un avviso di errore handshake irreversibile.

### <a name="BKMK_TrustedIssuers"></a>Gestione delle autorità emittenti attendibili per l'autenticazione client
Quando l'autenticazione del computer client è necessario con SSL o TLS, il server può essere configurato per l'invio di un elenco di autorità di certificazione attendibili. L'elenco contiene il set di autorità di certificazione che il server considererà attendibili e propone al computer client certificato client da selezionare se sono presenti più certificati. Inoltre, la catena di certificati che del computer client invia al server deve essere convalidata rispetto all'elenco di autorità emittenti attendibili configurate.

Prima di Windows Server 2012 e Windows 8, le applicazioni o i processi che SSP Schannel (inclusi HTTP.sys e IIS) potevano fornire un elenco di autorità emittenti attendibili supportate per l'autenticazione Client tramite un elenco dei certificati attendibili (CTL).

In Windows Server 2012 e Windows 8, sono state apportate modifiche al processo di autenticazione sottostante in modo che:

-   Gestione dell'elenco di autorità emittenti attendibili basata sull'elenco di scopi consentiti non è più supportata.

-   Il comportamento di invio dell'elenco di autorità emittenti attendibili per impostazione predefinita è disattivato: valore predefinito della chiave del Registro di sistema SendTrustedIssuerList è ora 0 (disattivato per impostazione predefinita) anziché 1.

-   Viene mantenuta la compatibilità con le versioni precedenti dei sistemi operativi Windows.

**Aggiunta questo valore?**

A partire da Windows Server 2012, l'uso di CTL è stato sostituito con un'implementazione basata sull'archivio certificati. Questo consente un maggiore familiarità della gestibilità tramite i cmdlet di gestione certificati esistenti del provider PowerShell, nonché gli strumenti della riga di comando come certutil.exe.

Anche se la dimensione massima dell'elenco di autorità di certificazione attendibili che supporti il SSP Schannel (16 KB) resta lo stesso come in Windows Server 2008 R2, in Windows Server 2012 è presente un certificato nuovo dedicato archivio per emittenti per l'autenticazione client in modo che i certificati non correlati non sono inclusi nel messaggio.

**Come funziona?**

In Windows Server 2012, l'elenco di autorità emittenti attendibili viene configurato usando gli archivi certificati; archivio di certificati computer globale predefinito e uno che è facoltativo per ogni sito. L'origine dell'elenco verrà determinata come segue:

-   Se è presente un archivio credenziali specifico configurato per il sito, verrà utilizzato come origine di

-   Se non esistono certificati nell'archivio definito dall'applicazione, quindi Schannel controlla il **emittenti per l'autenticazione Client** archivio del computer locale e, se sono presenti certificati, usa tale archivio come origine. Se non viene trovato alcun certificato in due archivi, viene controllato l'archivio delle radici attendibili.

-   Se nessuno dei due archivi locale o globale contengono certificati, il provider Schannel userà il **certificazione delle radici attendibili** archiviare come origine dell'elenco di autorità emittenti attendibili. (Questo è il comportamento per Windows Server 2008 R2).

Se il **autorità di certificazione delle radici attendibili radice** store che è stato usato contiene una combinazione di radice (autofirmati) e certificati di autorità (CA) emittente, verranno inviati al server solo i certificati di autorità di certificazione per impostazione predefinita.

**Come configurare Schannel per usare l'archivio certificati delle autorità emittenti attendibili**

L'architettura SSP Schannel in Windows Server 2012 utilizzerà per impostazione predefinita gli archivi come descritto in precedenza per gestire l'elenco di autorità emittenti attendibili. È comunque possibile utilizzare i cmdlet di gestione certificati esistenti del provider PowerShell, nonché gli strumenti della riga di comando, ad esempio Certutil per gestire i certificati.

For information about managing certificates using the PowerShell provider, see [AD CS Administration Cmdlets in Windows](https://technet.microsoft.com/library/hh848365(v=wps.620).aspx).

For information about managing certificates using the certificate utility, see [certutil.exe](https://technet.microsoft.com/library/cc732443.aspx).

For information about what data, including the application-defined store, is defined for an Schannel credential, see [SCHANNEL_CRED structure (Windows)](https://msdn.microsoft.com/library/windows/desktop/aa379810(v=vs.85).aspx).

**Impostazioni predefinite per le modalità di attendibilità**

Esistono tre modalità autenticazione Client attendibili supportate dal provider Schannel. The trust mode controls how validation of the client's certificate chain is performed and is a system-wide setting controlled by the REG_DWORD "ClientAuthTrustMode" under HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\Schannel.

|Valore|Modalità di attendibilità|Descrizione|
|-----|-------|--------|
|0|Machine Trust (impostazione predefinita)|Richiede che il certificato client venga emesso da un certificato nell'elenco di autorità emittenti attendibili.|
|1|Trust esclusivo radice|Richiede che un certificato client sia concatenato a un certificato radice contenuto nell'archivio di autorità emittenti attendibili specificato dal chiamante. Il certificato deve anche essere emesso da un'autorità emittente inclusa nell'elenco di autorità emittenti attendibili|
|2|Trust esclusivo autorità di certificazione|Richiede che una catena di certificati client da parte di un certificato della CA intermedia o certificato radice nel specificato dal chiamante trusted archivio Autorità di certificazione.|

For information about authentication failures due to trusted issuers configuration issues, see Knowledge Base article [280256](https://support.microsoft.com/kb/2802568).

### <a name="BKMK_SNI"></a>Supporto TLS per estensioni Server Name Indicator (SNI)
Funzionalità di indicazione nome server estende i protocolli SSL e TLS per consentire la corretta identificazione del server quando numerose immagini virtuali sono in esecuzione in un singolo server. Per proteggere la comunicazione tra un computer client e un server, il computer client richiede un certificato digitale dal server. Dopo che il server risponde alla richiesta e invia il certificato, il client esamina, viene utilizzato per crittografare le comunicazioni e procede con lo scambio di richiesta-risposta normale. Tuttavia, in uno scenario di hosting virtuale diversi domini, ognuno con il proprio certificato potenzialmente distinto, sono ospitati in un server. In questo caso, il server non ha modo di sapere in anticipo quale certificato per inviare al computer client. SNI consente al computer client di informare il dominio di destinazione in precedenza nel protocollo e questo consente al server di selezionare correttamente il certificato appropriato.

**Aggiunta questo valore?**

Questa funzionalità aggiuntiva:

-   Consente di ospitare più siti Web SSL su una combinazione di porta e IP singolo

-   Riduce l'utilizzo della memoria quando più siti Web SSL sono ospitati in un singolo server web.

-   Consente a più utenti per connettersi a siti Web SSL contemporaneamente

-   Consente di fornire suggerimenti agli utenti finali tramite l'interfaccia del computer per la selezione del certificato corretto durante un processo di autenticazione client.

**Come funziona**

SSP Schannel mantiene una cache in memoria degli stati di connessione client consentiti per i client. In questo modo i computer client possono riconnettersi al server SSL senza soggetti a un handshake SSL completo nelle visite successive.  Questo uso efficiente della gestione dei certificati consente di configurare ulteriori siti per essere ospitato in un singolo rispetto alle versioni precedenti del sistema operativo di Windows Server 2012.

Selezione del certificato dall'utente finale è stata migliorata perché consente di creare un elenco di nomi di probabili certificato dell'autorità di certificazione che forniscono all'utente finale suggerimenti su come quella da scegliere. Questo elenco è configurabile tramite criteri di gruppo.

### <a name="BKMK_DTLS"></a>Datagram Transport Layer Security (DTLS)
Il protocollo DTLS versione 1.0 è stato aggiunto al Security Support Provider Schannel. Il protocollo DTLS fornisce la riservatezza delle comunicazioni per i protocolli di datagramma. Il protocollo consente ai client/server applicazioni di comunicare in modo che è stato progettato per impedire l'intercettazione, la manomissione o la contraffazione dei messaggi. Il protocollo DTLS è basato sul protocollo Transport Layer Security (TLS) e fornisce garanzie di sicurezza equivalenti, riducendo la necessità di usare IPsec o di progettare un protocollo di sicurezza di livello dell'applicazione personalizzata.

**Aggiunta questo valore?**

I datagrammi sono comuni nei flussi multimediali, ad esempio le videoconferenze protette o i giochi. Aggiunta del protocollo DTLS al provider Schannel offre la possibilità di usare il noto modello Windows SSPI per proteggere la comunicazione tra computer client e server. DTLS è deliberatamente progettato per essere come simile a TLS possibili, sia per ridurre al minimo invenzione protezione nuovo e ottimizzare la quantità di riutilizzo del codice e dell'infrastruttura.

**Come funziona**

Le applicazioni che usano DTLS su UDP possono utilizzare il modello SSPI in Windows Server 2012 e Windows 8. Alcuni pacchetti di crittografia sono disponibili per la configurazione, simile a come è possibile configurare TLS. Schannel continua a utilizzare il provider di crittografia di CNG che sfrutta i vantaggi della certificazione FIPS 140, introdotta in Windows Vista.

### <a name="BKMK_Deprecated"></a>Funzionalità deprecate
In SSP Schannel per Windows Server 2012 e Windows 8, non esistono alcuna funzionalità o caratteristiche deprecate.

## <a name="see-also"></a>Vedere anche
-   [Modello di sicurezza del Cloud privato -: funzionalità Wrapper](https://social.technet.microsoft.com/wiki/contents/articles/6756.private-cloud-security-model-wrapper-functionality.aspx)



