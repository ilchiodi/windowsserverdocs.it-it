---
title: Architettura di interfaccia del Provider supporto sicurezza
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de09e099-5711-48f8-adbd-e7b8093a0336
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 8b0a74089c5d8a88a380f8a56e3b9e03b84081c1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="security-support-provider-interface-architecture"></a>Architettura di interfaccia del Provider supporto sicurezza

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento di riferimento per i professionisti IT descrive i protocolli di autenticazione di Windows che vengono utilizzati all'interno dell'architettura di Security Support Provider Interface (SSPI).

Microsoft Security Support Provider Interface (SSPI) è la base per l'autenticazione di Windows. Le applicazioni e servizi di infrastruttura che richiedono l'autenticazione usano SSPI per fornirla.

SSPI è l'implementazione del generico sicurezza servizio API (GSSAPI) nei sistemi operativi Windows Server. Per ulteriori informazioni su GSSAPI, vedere RFC 2743 e 2744 RFC nel Database IETF RFC.

L'impostazione predefinita Security Support Provider (SSP) che richiama i protocolli di autenticazione specifici in Windows sono incorporati in SSPI come DLL. Questi provider di servizi condivisi predefinito sono descritti nelle sezioni seguenti. Altri provider di servizi condivisi possono essere incorporate se operano con SSPI.

Come illustrato nell'immagine seguente, SSPI in Windows offre un meccanismo che esegue i token di autenticazione tramite il canale di comunicazione esistenti tra il computer client e il server. Quando due computer o dispositivi devono essere autenticati in modo che possano comunicare in modo sicuro, le richieste di autenticazione vengono indirizzate a SSPI, che completa il processo di autenticazione, indipendentemente dal protocollo di rete attualmente in uso. SSPI restituisce gli oggetti di grandi dimensioni trasparenti binari. Questi vengono passati tra le applicazioni, che possono essere passati al livello di SSPI. Pertanto, SSPI consente a un'applicazione da utilizzare vari modelli di sicurezza disponibili in un computer o una rete senza modificare l'interfaccia per il sistema di sicurezza.

![Diagramma che illustra l'architettura di interfaccia del Provider supporto sicurezza](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

Le sezioni seguenti descrivono il SSP predefinito che interagiscono con SSPI. Il provider di servizi condivisi vengono utilizzati in modi diversi nei sistemi operativi Windows per promuovere una comunicazione protetta in un ambiente di rete non sicura.

-   [Kerberos Security Support Provider](#BKMK_KerbSSP)

-   [NTLM Security Support Provider](#BKMK_NTLMSSP)

-   [Digest Security Support Provider](#BKMK_DigestSSP)

-   [Security Support Provider Schannel](#BKMK_SchannelSSP)

-   [Negoziare Security Support Provider](#BKMK_NegoSSP)

-   [Credential Security Support Provider](#BKMK_CredSSP)

-   [Negoziare estensioni Security Support Provider](#BKMK_NegoExtsSSP)

-   [PKU2U Security Support Provider](#BKMK_PKU2USSP)

Incluso anche in questo argomento:

[Selezione Provider supporto sicurezza](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Kerberos Security Support Provider
Questo provider di servizi condivisi Usa solo il protocollo Kerberos versione 5 come implementato da Microsoft. Questo protocollo è basato sul 4120 RFC e le revisioni bozza del gruppo di lavoro rete. È un protocollo standard che viene utilizzato con una password o una smart card per un accesso interattivo. È anche il metodo di autenticazione preferito per i servizi in Windows.

Poiché il protocollo Kerberos è stato il protocollo di autenticazione predefinito a partire da Windows 2000, tutti i servizi di dominio supportano SSP. Kerberos Tali servizi includono:

-   Active Directory Lightweight Directory Access Protocol (LDAP) che usano una query

-   Gestione remota server o workstation che utilizza il servizio Remote Procedure Call

-   Servizi di stampa

-   Autenticazione client-server

-   Accesso remoto ai file che utilizza il protocollo Server Message Block (SMB) (anche noto come Common Internet File System o CIFS)

-   Gestione di Distributed file system e di riferimento

-   Autenticazione Intranet a Internet Information Services (IIS)

-   Autenticazione di autorità di sicurezza per Internet Protocol security (IPsec)

-   Richieste di certificati per Servizi certificati di Active Directory per computer e utenti di dominio

Percorso: %windir%\Windows\System32\kerberos.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, e Windows Server 2003 e Windows XP.

**Risorse aggiuntive per il protocollo Kerberos e il SSP Kerberos**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: estensioni del protocollo Kerberos](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: estensioni del protocollo Kerberos: il servizio per utente e la delega vincolata specifica del protocollo](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [Kerberos SSP/AP (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Miglioramenti a Kerberos](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx) per Windows Vista

-   [Modifiche all'autenticazione Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) per Windows 7 

-   [Riferimento tecnico sull'autenticazione Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>NTLM Security Support Provider
NTLM Security Support Provider (SSP NTLM) è un file binario protocollo utilizzato dal Security Support Provider Interface (SSPI) per consentire l'autenticazione challenge / response NTLM e di negoziare le opzioni di integrità e riservatezza di messaggistica. NTLM viene utilizzato ogni volta che viene utilizzata autenticazione SSPI, incluso per l'autenticazione Server Message Block o CIFS autenticazione negoziare HTTP (ad esempio, l'autenticazione Web Internet) e il servizio Remote Procedure Call. SSP NTLM include il NTLM e NTLM versione 2 (NTLMv2) protocolli di autenticazione.

I sistemi operativi Windows supportati possibile usare SSP NTLM per gli elementi seguenti:

-   Autenticazione client/server

-   Servizi di stampa

-   Accesso ai file tramite CIFS (SMB)

-   Servizio Remote Procedure Call sicuro o DCOM

Percorso: %windir%\Windows\System32\msv1_0.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, e Windows Server 2003 e Windows XP.

**Risorse aggiuntive per il protocollo NTLM e SSP NTLM**

-   [Pacchetto di autenticazione Msv1_0 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [Modifiche all'autenticazione NTLM](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx) in Windows 7 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [Guida di utilizzo NTLM limitazione e al controllo](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>Digest Security Support Provider
L'autenticazione del digest è uno standard utilizzato per l'autenticazione web e Lightweight Directory Access Protocol (LDAP). L'autenticazione del digest trasmette le credenziali attraverso la rete come un digest messaggio o hash MD5.

Provider di servizi condivisi digest (Wdigest.dll) viene utilizzato per le operazioni seguenti:

-   Accesso a Internet Explorer e Internet Information Services (IIS)

-   Query LDAP

Percorso: %windir%\Windows\System32\Digest.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, e Windows Server 2003 e Windows XP.

**Risorse aggiuntive per il protocollo Digest e SSP del Digest**

-   [Autenticazione del Digest Microsoft (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-DPSP\]: estensioni del protocollo digest](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Security Support Provider Schannel
Il Secure Channel (Schannel) viene utilizzato per l'autenticazione basata su web server, ad esempio quando un utente tenta di accedere a un server web protetto.

Il protocollo TLS, il protocollo SSL, il protocollo Private Communications Technology (PCT) e il protocollo Datagram Transport Layer (DTLS) sono basati sulla crittografia a chiave pubblica. Schannel offre tutti questi protocolli. Tutti i protocolli Schannel usano un modello client/server. SSP Schannel Usa certificati a chiave pubblica per l'autenticazione delle entità. Quando l'autenticazione di entità, SSP Schannel seleziona un protocollo nell'ordine di preferenza seguente:

-   Transport Layer Security (TLS) versione 1.0

-   Transport Layer Security (TLS) versione 1.1

-   Transport Layer Security (TLS) versione 1.2

-   Secure Socket Layer (SSL) versione 2.0

-   Secure Socket Layer (SSL) versione 3.0

-   Private Communications Technology (PCT)

    **Nota** PCT è disabilitato per impostazione predefinita.

Il protocollo selezionato è il protocollo di autenticazione preferito che il client e il server può supportare. Ad esempio, se un server supporta tutti i protocolli Schannel e il client supporta solo SSL 3.0 e SSL 2.0, il processo di autenticazione Usa SSL 3.0.

DTLS viene utilizzato quando viene chiamato in modo esplicito dall'applicazione. Per ulteriori informazioni su DTLS e altri protocolli utilizzati dal provider Schannel, vedere [Schannel Security Support Provider Technical Reference](../tls/schannel-security-support-provider-technical-reference.md).

Percorso: %windir%\Windows\System32\Schannel.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, e Windows Server 2003 e Windows XP.

> [!NOTE]
> TLS 1.2 è stato introdotto in questo provider in Windows Server 2008 R2 e Windows 7. DTLS è stata introdotta in questo provider in Windows Server 2012 e Windows 8.

**Risorse aggiuntive per i protocolli TLS e SSL e SSP Schannel**

-   [Canale sicuro (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [Documentazione tecnica su TLS/SSL](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: transport Layer Security (TLS) profilo](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>Negoziare Security Support Provider
Il Simple and Protected GSS-API Negotiation meccanismo (SPNEGO) costituisce la base per il provider SSP Negotiate whichcan essere usati per negoziare un protocollo di autenticazione specifici. Quando un'applicazione chiama SSPI per accedere a una rete, è possibile specificare un provider di servizi condivisi per elaborare la richiesta. Se l'applicazione specifica il provider SSP Negotiate, analizza la richiesta e seleziona il provider appropriato per gestire la richiesta, in base ai criteri di sicurezza configurato al cliente.

SPNEGO è specificato nella RFC 2478.

Nelle versioni supportate dei sistemi operativi Windows, la sicurezza Negotiate supportano Seleziona provider tra il protocollo Kerberos e NTLM. Seleziona negoziazione del protocollo di Kerberos per impostazione predefinita, a meno che tale protocollo può essere utilizzato da uno dei sistemi di autenticazione o l'applicazione chiamante non ha fornito informazioni sufficienti per utilizzare il protocollo Kerberos.

Percorso: %windir%\Windows\System32\lsasrv.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, e Windows Server 2003 e Windows XP.

**Risorse aggiuntive per il provider SSP Negotiate**

-   [Microsoft negoziare (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: estensioni del semplice e protetto GSS-API negoziazione meccanismo (SPNEGO)](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: negoziare e l'autenticazione HTTP Nego2 specifica del protocollo](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>Credential Security Support Provider
Credential Security Service Provider (CredSSP) offre un'esperienza single sign-on (SSO) utente quando si avvia nuove sessioni Servizi Terminal e Servizi Desktop remoto. CredSSP consente alle applicazioni di delegare le credenziali degli utenti dal computer client (tramite il provider di servizi condivisi sul lato client) al server di destinazione (tramite sul lato server SSP), in base ai criteri del client. Criteri CredSSP vengono configurati tramite criteri di gruppo e la delega delle credenziali è disattivata per impostazione predefinita.

Percorso: %windir%\Windows\System32\credssp.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento.

**Risorse aggiuntive per il provider di credenziali**

-   [\[MS-CSSP\]: specifica del protocollo Security Support Provider (CredSSP) delle credenziali](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [Credential Security Service Provider e SSO per Terminal servizi di accesso](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>Negoziare estensioni Security Support Provider
Estensioni di negoziare (NegoExts) è un pacchetto di autenticazione che negozia l'uso di provider di servizi condivisi, diverso da NTLM o il protocollo Kerberos, per applicazioni e degli scenari implementato da Microsoft e altri software società.

Questa estensione per il pacchetto Negotiate consente gli scenari seguenti:

-   **Disponibilità di rich client all'interno di un sistema federativo.** Documenti accessibili nei siti di SharePoint e che possono essere modificati utilizzando un'applicazione di Microsoft Office complete.

-   **Rich client supporta per i servizi di Microsoft Office.** Gli utenti possono accedere ai servizi di Microsoft Office e utilizzare un'applicazione di Microsoft Office complete.

-   **Server ospitato Microsoft Exchange e Outlook.** Non esiste alcuna relazione di trust di dominio stabilita perché il Server Exchange ospitato sul web. Outlook utilizza il servizio Windows Live per autenticare gli utenti.

-   **Disponibilità di rich client tra i computer client e server.** Componenti di rete e di autenticazione del sistema operativo vengono utilizzati.

Il pacchetto Windows Negotiate considera SSP NegoExts esattamente come per l'autenticazione Kerberos e NTLM. NegoExts.dll viene caricato nel sistema autorità locale (LSA) all'avvio. Quando viene ricevuta una richiesta di autenticazione, in base a origine della richiesta, NegoExts negozia tra il provider di servizi condivisi supportati. Raccoglie le credenziali e i criteri, li crittografa e invia le informazioni al provider di servizi condivisi appropriate, in cui viene creato il token di sicurezza.

Il provider di servizi condivisi supportati da NegoExts non sono SSP autonomo, ad esempio Kerberos e NTLM. Di conseguenza, all'interno di SSP NegoExts, quando il metodo di autenticazione non riesce per qualsiasi motivo, un messaggio di errore di autenticazione verrà visualizzato o registrato. Metodi di autenticazione non rinegoziazione o di fallback sono possibili.

Percorso: %windir%\Windows\System32\negoexts.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, escluse Windows Server 2008 e Windows Vista.

### <a name="BKMK_PKU2USSP"></a>PKU2U Security Support Provider
Il protocollo PKU2U introdotto e implementato come un provider di servizi condivisi in Windows 7 e Windows Server 2008 R2. Questo provider di servizi condivisi Abilita l'autenticazione peer-to-peer, in particolare attraverso i supporti e funzionalità chiamata gruppo home, che è stata introdotta in Windows 7 di condivisione file. La funzionalità consente la condivisione tra computer che non sono membri di un dominio.

Percorso: %windir%\Windows\System32\pku2u.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, escluse Windows Server 2008 e Windows Vista.

**Risorse aggiuntive per il protocollo PKU2U e SSP PKU2U**

-   [Introduzione a integrazione delle identità in linea](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>Selezione Provider supporto sicurezza
Windows SSPI è possibile utilizzare i protocolli supportati tramite la Security Support Provider installati. Tuttavia, poiché non tutti i sistemi operativi supportano gli stessi pacchetti SSP come un determinato computer che esegue Windows Server, client e server devono negoziare l'uso di un protocollo di entrambi supportano. Windows Server preferenziale per i computer client e applicazioni di utilizzare il protocollo Kerberos, un protocollo sicuro basato su standard, quando possibile, ma il sistema operativo continua consentire alle applicazioni che non supportano il protocollo Kerberos per l'autenticazione di client e i computer client.

Prima di autenticazione può richiedere la comunicazione sul posto i due computer devono essere conformi al protocollo che entrambi possono supportare. Per qualsiasi protocollo siano utilizzabili tramite SSPI, ogni computer deve avere il SSP appropriato. Ad esempio, per un computer client e server da utilizzare il protocollo di autenticazione Kerberos, devono entrambe supportare Kerberos v5. Windows Server utilizza la funzione **EnumerateSecurityPackages** per identificare quali SSP sono supportati in un computer e quali sono le funzionalità di tali provider di servizi condivisi.

La selezione di un protocollo di autenticazione può essere gestita in uno dei due modi seguenti:

1.  [Solo protocollo di autenticazione](#BKMK_SingleAuth)

2.  [Opzione di negoziare](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>Solo protocollo di autenticazione
Quando un singolo protocollo accettabile viene specificato nel server, il computer client deve supportare il protocollo specificato o la comunicazione di ha esito negativo. Quando viene specificato un singolo protocollo accettabile, lo scambio di autenticazione viene eseguita come indicato di seguito:

1.  Il computer client richiede l'accesso a un servizio.

2.  Il server risponde alla richiesta e specifica il protocollo che verrà utilizzato.

3.  Il client esamina il contenuto della risposta e la verifica per determinare se supporta il protocollo specificato. Se il computer client supporta il protocollo specificato, l'autenticazione continua. Se il computer client non supporta il protocollo, l'autenticazione ha esito negativo, indipendentemente dal fatto che il computer client è autorizzato per accedere alla risorsa.

### <a name="BKMK_Negotiate"></a>Opzione di negoziare
L'opzione negotiate può essere utilizzata per consentire a client e server tentare di trovare un protocollo accettabile. Questo dipende il Simple and Protected GSS-API Negotiation meccanismo (SPNEGO). Quando l'autenticazione inizia con l'opzione per negoziare un protocollo di autenticazione, lo scambio SPNEGO avviene come segue:

1.  Il computer client richiede l'accesso a un servizio.

2.  Il server risponde con un elenco di protocolli di autenticazione in grado di supportare e una richiesta di autenticazione o la risposta, basato sul protocollo che è la prima opzione. Ad esempio, il server potrebbe elencare il protocollo Kerberos e NTLM e inviare una risposta di autenticazione Kerberos.

3.  Il client esamina il contenuto della risposta e la verifica per determinare se supporta i protocolli specificato.

    -   Se il computer client supporta il protocollo preferito, l'autenticazione procede.

    -   Se il computer client non supporta il protocollo preferito, ma supporta uno degli altri protocolli elencati dal server, il computer client informa il server che supporta il protocollo di autenticazione e l'autenticazione procede.

    -   Se il computer client non supporta i protocolli elencati, lo scambio di autenticazione non riesce.

## <a name="see-also"></a>Vedere anche
[Architettura dell'autenticazione di Windows](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


