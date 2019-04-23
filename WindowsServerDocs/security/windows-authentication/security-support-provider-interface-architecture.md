---
title: Architettura dell'interfaccia del provider supporto Sicurezza
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827022"
---
# <a name="security-support-provider-interface-architecture"></a>Architettura dell'interfaccia del provider supporto Sicurezza

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento di riferimento per i professionisti IT descrive i protocolli di autenticazione di Windows che vengono utilizzati all'interno dell'architettura di Security Support Provider Interface (SSPI).

Microsoft Security Support Provider Interface (SSPI) è la base per l'autenticazione di Windows. Le applicazioni e servizi di infrastruttura che richiedono l'autenticazione usano SSPI per fornirla.

SSPI è l'implementazione del generico sicurezza del servizio API GSSAPI () nei sistemi operativi Windows Server. Per altre informazioni su GSSAPI, vedere RFC 2743 e RFC 2744 nel Database IETF RFC.

L'impostazione predefinita Security Support Provider (SSP) che richiamano i protocolli di autenticazione specifico in Windows sono incorporati nel SSPI sotto forma di DLL. Questi provider di servizi condivisi predefiniti sono descritte nelle sezioni seguenti. Altri provider di servizi condivisi possono essere incorporate se possono operare con l'interfaccia SSPI.

Come illustrato nell'immagine seguente, l'interfaccia SSPI di Windows fornisce un meccanismo che trasporta i token di autenticazione tramite il canale di comunicazione esistenti tra il computer client e il server. Quando due computer o dispositivi devono essere autenticati in modo che possano comunicare in modo sicuro, le richieste di autenticazione vengono indirizzate a SSPI, che completa il processo di autenticazione, indipendentemente dal protocollo di rete attualmente in uso. SSPI restituisce trasparenti oggetti binari di grandi dimensioni. Questi vengono passati tra le applicazioni, a quel punto possono essere passati a livello SSPI. Di conseguenza, l'interfaccia SSPI consente a un'applicazione usare diversi modelli di sicurezza disponibili in un computer o una rete senza modificare l'interfaccia per il sistema di sicurezza.

![Diagramma che mostra l'architettura dell'interfaccia Provider supporto sicurezza](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

Le sezioni seguenti descrivono il SSP predefiniti che interagiscono con l'interfaccia SSPI. Il provider di servizi condivisi vengono utilizzati in modi diversi nei sistemi operativi Windows a promuovere comunicazioni sicure in un ambiente di rete non sicura.

-   [Provider supporto sicurezza Kerberos](#BKMK_KerbSSP)

-   [Provider supporto protezione LM NT](#BKMK_NTLMSSP)

-   [Digest Security Support Provider](#BKMK_DigestSSP)

-   [Security Support Provider Schannel](#BKMK_SchannelSSP)

-   [Negoziazione Security Support Provider](#BKMK_NegoSSP)

-   [Credential Security Support Provider](#BKMK_CredSSP)

-   [Negoziazione Security Support Provider di estensioni](#BKMK_NegoExtsSSP)

-   [Provider supporto sicurezza PKU2U](#BKMK_PKU2USSP)

Incluso anche in questo argomento:

[Selezione del Provider supporto sicurezza](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Provider supporto sicurezza Kerberos
Questo provider di servizi condivisi Usa solo il protocollo Kerberos versione 5 come implementato da Microsoft. Questo protocollo è basato su RFC 4120 del gruppo di lavoro rete e le revisioni di draft. È un protocollo standard che viene usato con una password o una smart card per un accesso interattivo. È anche il metodo di autenticazione preferito per i servizi in Windows.

Poiché il protocollo Kerberos è stato il protocollo di autenticazione predefinito a partire da Windows 2000, tutti i servizi di dominio supportano SSP. Kerberos I servizi inclusi sono i seguenti:

-   Active Directory esegue query che utilizzano Lightweight Directory Access Protocol (LDAP)

-   Gestione server o una workstation remota che utilizza il servizio Remote Procedure Call

-   Servizi di stampa

-   Autenticazione client-server

-   Accesso remoto ai file che utilizza il protocollo Server Message Block (SMB) (anche noto come CIFS o Common Internet File System)

-   Gestione di Distributed file system e di riferimento

-   Autenticazione Intranet a Internet Information Services (IIS)

-   Autenticazione dell'autorità di sicurezza per Internet Protocol security (IPsec)

-   Richieste di certificati per Servizi certificati di Active Directory per computer e utenti del dominio

Percorso: %windir%\Windows\System32\kerberos.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, oltre a Windows Server 2003 e Windows XP.

**Risorse aggiuntive per il protocollo Kerberos e il SSP Kerberos**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: Estensioni del protocollo Kerberos](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: estensioni del protocollo Kerberos: Servizio per utente e la specifica del protocollo di delega vincolata](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [Kerberos SSP/AP (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Miglioramenti a Kerberos](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx) per Windows Vista

-   [Modifiche all'autenticazione Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) per Windows 7 

-   [Riferimento tecnico sull'autenticazione di Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>Provider supporto protezione LM NT
Il Provider supporto protezione LM NT (NTLM SSP) è un file binario messaggistica protocollo usato dal Security Support Provider Interface (SSPI) per consentire l'autenticazione NTLM challenge / response e negoziare le opzioni di integrità e riservatezza. NTLM viene utilizzato ogni volta che viene utilizzata l'autenticazione SSPI, tra cui per l'autenticazione di Server Message Block o CIFS, l'autenticazione Negotiate HTTP (ad esempio, l'autenticazione Web Internet) e il servizio Remote Procedure Call. NTLM SSP include il NTLM ed NTLM versione 2 (NTLMv2) i protocolli di autenticazione.

I sistemi operativi Windows supportati è possibile usare SSP NTLM per gli elementi seguenti:

-   Autenticazione client/server

-   Servizi di stampa

-   Accesso ai file tramite CIFS (SMB)

-   Secure servizio Remote Procedure Call o DCOM

Percorso: %windir%\Windows\System32\msv1_0.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, oltre a Windows Server 2003 e Windows XP.

**Risorse aggiuntive per il protocollo NTLM e NTLM SSP**

-   [Pacchetto di autenticazione Msv1_0 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [Modifiche all'autenticazione NTLM](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx) in Windows 7 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [Guida all'utilizzo NTLM di limitazione e al controllo](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>Digest Security Support Provider
L'autenticazione del digest è uno standard del settore che viene usato per l'autenticazione web e Lightweight Directory Access Protocol (LDAP). L'autenticazione del digest trasmette le credenziali attraverso la rete come un digest MD5 hash o un messaggio.

Digest SSP (Wdigest.dll) viene usato per le operazioni seguenti:

-   Accesso a Internet Explorer e Internet Information Services (IIS)

-   Query LDAP

Percorso: %windir%\Windows\System32\Digest.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, oltre a Windows Server 2003 e Windows XP.

**Risorse aggiuntive per il protocollo di Digest e SSP Digest**

-   [Autenticazione Digest Microsoft (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-DPSP\]: Estensioni del protocollo del digest](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Security Support Provider Schannel
Secure Channel (Schannel) viene usato per l'autenticazione server basata sul web, ad esempio quando un utente tenta di accedere a un server web protetto.

Il protocollo TLS, il protocollo SSL, il protocollo Private Communications Technology (PCT) e il protocollo Datagram Transport Layer (DTLS) sono basati sulla crittografia a chiave pubblica. Schannel offre tutti questi protocolli. Tutti i protocolli Schannel usano un modello client/server. SSP Schannel usa certificati a chiave pubblica per l'autenticazione delle entità. L'autenticazione di entità, SSP Schannel seleziona un protocollo nel seguente ordine di preferenza:

-   Trasporto Layer Security (TLS) versione 1.0

-   Trasporto Layer Security (TLS) versione 1.1

-   Trasporto Layer Security (TLS) versione 1.2

-   Secure Socket Layer (SSL) versione 2.0

-   Secure Socket Layer (SSL) versione 3.0

-   Private Communications Technology (PCT)

    **Nota** PCT è disabilitata per impostazione predefinita.

Il protocollo selezionato è il protocollo di autenticazione preferito che il client e il server può supportare. Ad esempio, se un server supporta tutti i protocolli Schannel e il client supporta solo SSL 3.0 e 2.0 di SSL, il processo di autenticazione Usa SSL 3.0.

DTLS viene usato quando viene chiamato in modo esplicito dall'applicazione. Per altre informazioni sulle DTLS e gli altri protocolli utilizzati dal provider Schannel, vedere [riferimento tecnico Provider supporto sicurezza Schannel](../tls/schannel-security-support-provider-technical-reference.md).

Percorso: %windir%\Windows\System32\Schannel.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, oltre a Windows Server 2003 e Windows XP.

> [!NOTE]
> TLS 1.2 è stato introdotto nel provider in Windows Server 2008 R2 e Windows 7. DTLS è stato introdotto nel provider in Windows Server 2012 e Windows 8.

**Risorse aggiuntive per i protocolli TLS e SSL e SSP Schannel**

-   [Canale sicuro (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [Riferimento tecnico di TLS/SSL](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: Transport Layer Security (TLS) Profile](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>Negoziazione Security Support Provider
Il Simple and Protected GSS-API Negotiation meccanismo (SPNEGO) costituisce la base per il provider SSP Negotiate, whichcan utilizzabile per negoziare un protocollo di autenticazione specifico. Quando un'applicazione chiama l'interfaccia SSPI per accedere a una rete, è possibile specificare un provider SSP per elaborare la richiesta. Se l'applicazione specifica il provider SSP Negotiate, analizza la richiesta e seleziona il provider appropriato per gestire la richiesta, in base ai criteri di sicurezza configurato dal cliente.

SPNEGO è specificato in RFC 2478.

Nelle versioni supportate dei sistemi operativi Windows, la sicurezza Negotiate supportano Seleziona provider tra il protocollo Kerberos e NTLM. La negoziazione consente di selezionare il protocollo Kerberos per impostazione predefinita, a meno che tale protocollo non può essere utilizzato da uno dei sistemi coinvolti nell'autenticazione o l'applicazione chiamante non ha fornito informazioni sufficienti per usare il protocollo Kerberos.

Percorso: %windir%\Windows\System32\lsasrv.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, oltre a Windows Server 2003 e Windows XP.

**Risorse aggiuntive per il provider SSP Negotiate**

-   [Microsoft Negotiate (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: Estensioni di semplici e Protected GSS-API negoziazione meccanismo (SPNEGO)](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: Negotiate e l'autenticazione HTTP Nego2 specifica del protocollo](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>Credential Security Support Provider
Credential Security Service Provider (CredSSP) offre un'esperienza single sign-on (SSO) utente all'avvio di nuove sessioni di servizi Terminal e Servizi Desktop remoto. CredSSP consente alle applicazioni di delegare le credenziali degli utenti dal computer client (tramite SSP lato client) al server di destinazione (tramite SSP) sul lato server, in base ai criteri del client. I criteri CredSSP vengono configurati tramite criteri di gruppo e la delega delle credenziali è stata disattivata per impostazione predefinita.

Percorso: %windir%\Windows\System32\credssp.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento.

**Risorse aggiuntive per il provider di credenziali**

-   [\[MS-CSSP\]: Specifica del protocollo Credential Security Support Provider (CredSSP)](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [Credential Security Support Provider e l'accesso SSO per Terminal Services Logon](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>Negoziazione Security Support Provider di estensioni
La negoziazione delle estensioni (NegoExts) è un pacchetto di autenticazione che negozia l'uso di provider di servizi condivisi, diverso da NTLM o il protocollo Kerberos, per applicazioni e gli scenari implementato da Microsoft e altri software alle aziende.

Questa estensione per il pacchetto di negoziazione consente gli scenari seguenti:

-   **Disponibilità di rich client all'interno di un sistema federato.** I documenti sono accessibili nei siti di SharePoint e può essere modificati tramite un'applicazione di Microsoft Office complete.

-   **Rich client supporta per i servizi di Microsoft Office.** Gli utenti possono accedere ai servizi di Microsoft Office e usare un'applicazione di Microsoft Office complete.

-   **Microsoft Exc_hange Server ospitato e Outlook.** Non è presente alcun trust di dominio stabilita perché il Server di Exchange è ospitato sul web. Outlook Usa il servizio Windows Live per l'autenticazione degli utenti.

-   **Disponibilità di rich client tra i computer client e server.** Componenti di rete e l'autenticazione del sistema operativo vengono usati.

Il pacchetto di Windows Negotiate considera SSP NegoExts esattamente come per NTLM e Kerberos. NegoExts.dll viene caricato nel sistema autorità locale (LSA) all'avvio. Quando viene ricevuta una richiesta di autenticazione, basata sull'origine della richiesta, NegoExts negoziatore tra il SSP supportati. Raccoglie le credenziali e i criteri, li crittografa e invia le informazioni al provider di servizi condivisi appropriato, in cui viene creato il token di sicurezza.

il SSP supportati da NegoExts nejsou SSP autonoma, ad esempio Kerberos e NTLM. Di conseguenza, all'interno di SSP NegoExts, quando il metodo di autenticazione non riesce per qualsiasi motivo, un messaggio di errore di autenticazione verrà visualizzato o registrato. Nessun metodo di autenticazione rinegoziazione o di fallback è possibile.

Percorso: %windir%\Windows\System32\negoexts.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, ad eccezione di Windows Server 2008 e Windows Vista.

### <a name="BKMK_PKU2USSP"></a>Provider supporto sicurezza PKU2U
Il protocollo PKU2U introdotto e implementato come un provider di servizi condivisi in Windows 7 e Windows Server 2008 R2. Questo provider di servizi condivisi Abilita l'autenticazione peer-to-peer, in particolare con i supporti e una funzionalità chiamata gruppo home, che è stata introdotta in Windows 7 di condivisione file. La funzionalità consente la condivisione tra i computer che non sono membri di un dominio.

Percorso: %windir%\Windows\System32\pku2u.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nel **si applica a** elenco all'inizio di questo argomento, ad eccezione di Windows Server 2008 e Windows Vista.

**Risorse aggiuntive per il protocollo PKU2U e SSP PKU2U**

-   [Introduzione a integrazione di identità Online](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>Selezione del Provider supporto sicurezza
L'interfaccia SSPI di Windows è possibile usare uno dei protocolli supportati tramite il provider supporto sicurezza installati. Tuttavia, poiché non tutti i sistemi operativi supportano gli stessi pacchetti SSP come qualsiasi computer specificato che esegue Windows Server, client e server devono negoziare un protocollo supportato da entrambe. Windows Server preferenziale per i computer client e alle applicazioni di utilizzare il protocollo Kerberos, un protocollo sicuro basato su standard, quando possibile, ma il sistema operativo continua consentire alle applicazioni che non supportano il Kerberos di client e i computer client protocollo per l'autenticazione.

L'autenticazione per poter rendere comunicazione sul posto i due computer devono concordare un protocollo che può supportare entrambi. Per qualsiasi protocollo per poter essere usato tramite l'interfaccia SSPI, ogni computer deve avere il provider appropriato. Ad esempio, per un computer client e server per usare il protocollo di autenticazione Kerberos, devono entrambi supportare Kerberos v5. Windows Server viene utilizzata la funzione **EnumerateSecurityPackages** per identificare quali SSP sono supportati in un computer e che cosa sono le funzionalità di questi provider di servizi condivisi.

La selezione di un protocollo di autenticazione può essere gestita in uno dei due modi seguenti:

1.  [Protocollo di autenticazione Single](#BKMK_SingleAuth)

2.  [La negoziazione di opzione](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>Protocollo di autenticazione Single
Quando un singolo protocollo accettabile è specificato nel server, il computer client deve supportare il protocollo specificato o la comunicazione ha esito negativo. Quando viene specificato un unico protocollo accettabile, lo scambio di autenticazione viene eseguita come indicato di seguito:

1.  Il computer client richiede l'accesso a un servizio.

2.  Il server risponde alla richiesta e specifica il protocollo che verrà utilizzato.

3.  Il client esamina il contenuto della risposta e si verifica per determinare se supporta il protocollo specificato. Se il computer client supporta il protocollo specificato, l'autenticazione continua. Se il computer client non supporta il protocollo, l'autenticazione ha esito negativo, indipendentemente dal fatto che il computer client è autorizzato ad accedere alla risorsa.

### <a name="BKMK_Negotiate"></a>La negoziazione di opzione
L'opzione negotiate è utilizzabile per consentire al client e server di tentare di trovare un protocollo accettabile. Si basa il Simple and Protected GSS-API Negotiation meccanismo (SPNEGO). Quando viene avviata l'autenticazione con l'opzione per la negoziazione di un protocollo di autenticazione, il tunneling dello scambio SPNEGO viene eseguita come indicato di seguito:

1.  Il computer client richiede l'accesso a un servizio.

2.  Il server risponde con un elenco di protocolli di autenticazione che può supportare e una richiesta di autenticazione o come risposta basata sul protocollo che è la prima scelta. Ad esempio, il server potrebbe elencare il protocollo Kerberos e NTLM e inviare una risposta di autenticazione Kerberos.

3.  Il client esamina il contenuto della risposta e si verifica per determinare se supporta i protocolli specificati.

    -   Se il computer client supporta il protocollo preferito, l'autenticazione procede.

    -   Se il computer client non supporta il protocollo preferito, ma supporta uno degli altri protocolli elencate per il server, il client consente al server di sapere che supporta il protocollo di autenticazione e l'autenticazione procede.

    -   Se il computer client non supporta i protocolli elencate, lo scambio di autenticazione ha esito negativo.

## <a name="see-also"></a>Vedere anche
[Architettura di autenticazione di Windows](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


