---
title: Architettura dell'interfaccia del provider supporto Sicurezza
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4db407b24b00bc8313d2e17f1fcf55d9fa160c8c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403303"
---
# <a name="security-support-provider-interface-architecture"></a>Architettura dell'interfaccia del provider supporto Sicurezza

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento di riferimento per i professionisti IT descrive i protocolli di autenticazione di Windows utilizzati all'interno dell'architettura SSPI (Security Support Provider Interface).

Microsoft Security Support Provider Interface (SSPI) è la base per l'autenticazione di Windows. Le applicazioni e i servizi di infrastruttura che richiedono l'autenticazione usano SSPI per fornirli.

SSPI è l'implementazione di Generic Security Service API (GSSAPI) nei sistemi operativi Windows Server. Per ulteriori informazioni su GSSAPI, vedere RFC 2743 e RFC 2744 nel database IETF RFC.

Gli SSP (Security Support Provider) predefiniti che richiamano protocolli di autenticazione specifici in Windows sono incorporati in SSPI come dll. Questi SSP predefiniti sono descritti nelle sezioni seguenti. È possibile incorporare altri SSP se possono funzionare con SSPI.

Come illustrato nell'immagine seguente, SSPI in Windows fornisce un meccanismo che trasporta i token di autenticazione sul canale di comunicazione esistente tra il computer client e il server. Quando è necessario autenticare due computer o dispositivi affinché possano comunicare in modo sicuro, le richieste di autenticazione vengono indirizzate a SSPI, che completa il processo di autenticazione, indipendentemente dal protocollo di rete attualmente in uso. SSPI restituisce oggetti binari di grandi dimensioni trasparenti. Questi vengono passati tra le applicazioni e a questo punto possono essere passati al livello SSPI. In questo modo, SSPI consente a un'applicazione di usare diversi modelli di sicurezza disponibili in un computer o in una rete senza modificare l'interfaccia del sistema di sicurezza.

![Diagramma che illustra l'architettura dell'interfaccia del provider di supporto per la sicurezza](../media/security-support-provider-interface-architecture/AuthN_SecuritySupportProviderInterfaceArchitecture.jpg)

Le sezioni seguenti descrivono gli SSP predefiniti che interagiscono con SSPI. I provider di servizi condivisi vengono utilizzati in modi diversi nei sistemi operativi Windows per promuovere la comunicazione sicura in un ambiente di rete non sicuro.

-   [Security Support Provider Kerberos](#BKMK_KerbSSP)

-   [Security Support Provider NTLM](#BKMK_NTLMSSP)

-   [Provider di supporto per la sicurezza del digest](#BKMK_DigestSSP)

-   [Security Support Provider Schannel](#BKMK_SchannelSSP)

-   [Negozia provider di supporto per la sicurezza](#BKMK_NegoSSP)

-   [Provider di supporto per la sicurezza delle credenziali](#BKMK_CredSSP)

-   [Provider di supporto della sicurezza di Negotiate Extensions](#BKMK_NegoExtsSSP)

-   [PKU2U Security Support Provider](#BKMK_PKU2USSP)

Incluso anche in questo argomento:

[Selezione del provider di supporto per la sicurezza](security-support-provider-interface-architecture.md#BKMK_SecuritySupportProviderSelection)

### <a name="BKMK_KerbSSP"></a>Security Support Provider Kerberos
Questo SSP utilizza solo il protocollo Kerberos versione 5 implementato da Microsoft. Questo protocollo si basa sulle revisioni RFC 4120 e Draft del gruppo di lavoro di rete. Si tratta di un protocollo standard del settore usato con una password o una smart card per un accesso interattivo. È anche il metodo di autenticazione preferito per i servizi in Windows.

Poiché il protocollo Kerberos è stato il protocollo di autenticazione predefinito a partire da Windows 2000, tutti i servizi del dominio supportano l'SSP Kerberos. I servizi inclusi sono i seguenti:

-   Active Directory query che utilizzano LDAP (Lightweight Directory Access Protocol)

-   Gestione remota del server o della workstation che utilizza il servizio RPC (Remote Procedure Call)

-   Servizi di stampa

-   Autenticazione client-server

-   Accesso al file remoto che usa il protocollo SMB (Server Message Block), noto anche come CIFS (Common Internet file System)

-   Gestione e riferimento file system distribuiti

-   Autenticazione Intranet per Internet Information Services (IIS)

-   Autenticazione dell'autorità di sicurezza per Internet Protocol Security (IPsec)

-   Richieste di certificati per Active Directory Servizi certificati per utenti e computer di dominio

Percorso:%windir%\Windows\System32\kerberos.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nell'elenco **si applica a** all'inizio di questo argomento, oltre a windows Server 2003 e Windows XP.

**Risorse aggiuntive per il protocollo Kerberos e il provider di servizi condivisi Kerberos**

-   [Microsoft Kerberos (Windows)](https://msdn.microsoft.com/library/aa378747(VS.85).aspx)

-   [\[MS-KILE\]: estensioni del protocollo Kerberos](https://msdn.microsoft.com/library/cc233855(PROT.10).aspx)

-   [\[MS-SFU\]: estensioni del protocollo Kerberos: specifica del protocollo per il servizio per utenti e la delega vincolata](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx)

-   [SSP/AP Kerberos (Windows)](https://msdn.microsoft.com/library/aa377942(VS.85).aspx)

-   [Miglioramenti di Kerberos](https://technet.microsoft.com/library/cc749438(v=ws.10).aspx) per Windows Vista

-   [Modifiche all'autenticazione Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) per Windows 7 

-   [Riferimento tecnico per l'autenticazione Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)

### <a name="BKMK_NTLMSSP"></a>Security Support Provider NTLM
NTLM Security Support Provider (NTLM SSP) è un protocollo di messaggistica binario utilizzato da Security Support Provider Interface (SSPI) per consentire l'autenticazione di tipo richiesta-risposta NTLM e per negoziare le opzioni di integrità e riservatezza. L'autenticazione NTLM viene utilizzata in qualsiasi punto in cui viene utilizzata l'autenticazione SSPI, ad esempio per l'autenticazione Server Message Block o CIFS, l'autenticazione di negoziazione HTTP, ad esempio l'autenticazione Web Internet, e il servizio RPC (Remote Procedure Call). Il provider di servizi condivisi NTLM include i protocolli di autenticazione NTLM e NTLM versione 2 (NTLMv2).

I sistemi operativi Windows supportati possono utilizzare il provider di servizi condivisi NTLM per gli elementi seguenti:

-   Autenticazione client/server

-   Servizi di stampa

-   Accesso ai file tramite CIFS (SMB)

-   Servizio RPC (Remote Procedure Call) o servizio DCOM sicuro

Percorso:%windir%\Windows\System32\ msv1_0. dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nell'elenco **si applica a** all'inizio di questo argomento, oltre a windows Server 2003 e Windows XP.

**Risorse aggiuntive per il protocollo NTLM e il provider di servizi condivisi NTLM**

-   [Pacchetto di autenticazione MSV1_0 (Windows)](https://msdn.microsoft.com/library/aa378753(VS.85).aspx)

-   [Modifiche all'autenticazione NTLM](https://technet.microsoft.com/library/dd566199(v=ws.10).aspx) in Windows 7 

-   [Microsoft NTLM (Windows)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)

-   [Guida alla limitazione e al controllo dell'utilizzo di NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

### <a name="BKMK_DigestSSP"></a>Provider di supporto per la sicurezza del digest
L'autenticazione del digest è uno standard di settore usato per LDAP (Lightweight Directory Access Protocol) e l'autenticazione Web. L'autenticazione del digest trasmette le credenziali attraverso la rete come hash MD5 o digest del messaggio.

SSP digest (wdigest. dll) viene usato per le operazioni seguenti:

-   Accesso a Internet Explorer e Internet Information Services (IIS)

-   Query LDAP

Percorso:%windir%\Windows\System32\Digest.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nell'elenco **si applica a** all'inizio di questo argomento, oltre a windows Server 2003 e Windows XP.

**Risorse aggiuntive per il protocollo digest e il provider di servizi condivisi del digest**

-   [Autenticazione Microsoft Digest (Windows)](https://msdn.microsoft.com/library/aa378745(VS.85).aspx)

-   [\[MS-DPSP\]: estensioni del protocollo digest](https://msdn.microsoft.com/library/cc227906(PROT.13).aspx)

### <a name="BKMK_SchannelSSP"></a>Security Support Provider Schannel
Il canale sicuro (Schannel) viene utilizzato per l'autenticazione server basata sul Web, ad esempio quando un utente tenta di accedere a un server Web protetto.

Il protocollo TLS, il protocollo SSL, il protocollo PCT (Private Communications Technology) e il protocollo DTLS (Datagram Transport Layer) sono basati sulla crittografia a chiave pubblica. Schannel fornisce tutti questi protocolli. Tutti i protocolli Schannel usano un modello client/server. SSP Schannel usa certificati a chiave pubblica per l'autenticazione delle entità. Durante l'autenticazione delle entità, SSP Schannel seleziona un protocollo nell'ordine di preferenza seguente:

-   Transport Layer Security (TLS) versione 1,0

-   Transport Layer Security (TLS) versione 1,1

-   Transport Layer Security (TLS) versione 1,2

-   Secure Socket Layer (SSL) versione 2,0

-   Secure Socket Layer (SSL) versione 3,0

-   Tecnologia di comunicazione privata (PCT)

    **Nota** PCT è disabilitato per impostazione predefinita.

Il protocollo selezionato è il protocollo di autenticazione preferito che il client e il server possono supportare. Ad esempio, se un server supporta tutti i protocolli Schannel e il client supporta solo SSL 3,0 e SSL 2,0, il processo di autenticazione Usa SSL 3,0.

DTLS viene usato quando viene chiamato in modo esplicito dall'applicazione. Per ulteriori informazioni su DTLS e sugli altri protocolli utilizzati dal provider Schannel, vedere informazioni di [riferimento tecnico sul provider di supporto per la sicurezza Schannel](../tls/schannel-security-support-provider-technical-reference.md).

Percorso:%windir%\Windows\System32\Schannel.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nell'elenco **si applica a** all'inizio di questo argomento, oltre a windows Server 2003 e Windows XP.

> [!NOTE]
> TLS 1,2 è stato introdotto in questo provider in Windows Server 2008 R2 e Windows 7. DTLS è stato introdotto in questo provider in Windows Server 2012 e Windows 8.

**Risorse aggiuntive per i protocolli TLS e SSL e SSP Schannel**

-   [Canale sicuro (Windows)](https://msdn.microsoft.com/library/aa380123(VS.85).aspx)

-   [Riferimento tecnico per TLS/SSL](https://technet.microsoft.com/library/cc784149(v=ws.10).aspx)

-   [\[MS-TLSP\]: profilo Transport Layer Security (TLS)](https://msdn.microsoft.com/library/dd207968(PROT.13).aspx)

### <a name="BKMK_NegoSSP"></a>Negozia provider di supporto per la sicurezza
Il meccanismo SPNEGO (Simple and Protected GSS-API Negotiation Mechanism) costituisce la base per Negotiate SSP, whichcan essere utilizzato per negoziare un protocollo di autenticazione specifico. Quando un'applicazione esegue una chiamata a SSPI per accedere a una rete, può specificare un SSP per elaborare la richiesta. Se l'applicazione specifica il SSP Negotiate, analizza la richiesta e sceglie il provider appropriato per gestire la richiesta, in base ai criteri di sicurezza configurati dal cliente.

SPNEGO è specificato nella RFC 2478.

Nelle versioni supportate dei sistemi operativi Windows, il Security Support Provider Negotiate seleziona tra il protocollo Kerberos e NTLM. Negotiate seleziona il protocollo Kerberos per impostazione predefinita, a meno che tale protocollo non possa essere utilizzato da uno dei sistemi interessati dall'autenticazione oppure se l'applicazione chiamante non ha fornito informazioni sufficienti per utilizzare il protocollo Kerberos.

Percorso:%windir%\Windows\System32\lsasrv.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nell'elenco **si applica a** all'inizio di questo argomento, oltre a windows Server 2003 e Windows XP.

**Risorse aggiuntive per Negotiate SSP**

-   [Negozi Microsoft (Windows)](https://msdn.microsoft.com/library/aa378748(VS.85).aspx)

-   [\[MS-SPNG\]: estensioni SPNEGO (Simple and Protected GSS-API Negotiation Mechanism)](https://msdn.microsoft.com/library/cc247021(PROT.13).aspx)

-   [\[MS-N2HT\]: specifica del protocollo di autenticazione HTTP Negotiate e Nego2](https://msdn.microsoft.com/library/dd303576(PROT.13).aspx)

### <a name="BKMK_CredSSP"></a>Provider di supporto per la sicurezza delle credenziali
Il provider di servizi di sicurezza delle credenziali (CredSSP) fornisce un'esperienza utente Single Sign-On (SSO) per l'avvio di nuove sessioni di Servizi terminal e di Servizi Desktop remoto. CredSSP consente alle applicazioni di delegare le credenziali degli utenti dal computer client (tramite il provider di servizi condivisi sul lato client) al server di destinazione (tramite il provider di servizi condivisi sul lato server), in base ai criteri del client. I criteri CredSSP vengono configurati tramite Criteri di gruppo e la delega delle credenziali è disattivata per impostazione predefinita.

Percorso:%windir%\Windows\System32\credssp.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nell'elenco **si applica a** all'inizio di questo argomento.

**Risorse aggiuntive per le credenziali SSP**

-   [\[MS-CSSP\]: specifica del protocollo CredSSP (Credential Security Support Provider)](https://msdn.microsoft.com/library/cc226764(PROT.13).aspx)

-   [Provider di servizi di sicurezza delle credenziali e SSO per l'accesso a Servizi terminal](https://technet.microsoft.com/library/cc749211(v=ws.10).aspx)

### <a name="BKMK_NegoExtsSSP"></a>Provider di supporto della sicurezza di Negotiate Extensions
Negotiate Extensions (NegoExts) è un pacchetto di autenticazione che negozia l'uso di SSP, oltre a NTLM o al protocollo Kerberos, per le applicazioni e gli scenari implementati da Microsoft e altre società di software.

Questa estensione per il pacchetto Negotiate consente gli scenari seguenti:

-   **Disponibilità avanzata del client all'interno di un sistema federato.** È possibile accedere ai documenti nei siti di SharePoint e modificarli usando un'applicazione Microsoft Office completa.

-   **Supporto rich client per Microsoft Office Services.** Gli utenti possono accedere a Microsoft Office Services e usare un'applicazione Microsoft Office completa.

-   **Microsoft Exchange Server e Outlook ospitati.** Nessun trust di dominio stabilito perché Exchange Server è ospitato sul Web. Outlook usa il servizio Windows Live per autenticare gli utenti.

-   **Disponibilità avanzata del client tra computer client e server.** Vengono utilizzati i componenti di rete e di autenticazione del sistema operativo.

Il pacchetto di negoziazione di Windows considera il provider di servizi condivisi NegoExts nello stesso modo in cui avviene per Kerberos e NTLM. NegoExts. dll viene caricato nell'autorità di sistema locale (LSA) all'avvio. Quando viene ricevuta una richiesta di autenticazione, in base all'origine della richiesta, NegoExts negozia tra i provider di servizi condivisi supportati. Raccoglie le credenziali e i criteri, li crittografa e invia le informazioni all'SSP appropriato, in cui viene creato il token di sicurezza.

Gli SSP supportati da NegoExts non sono SSP autonomi, ad esempio Kerberos e NTLM. Pertanto, all'interno del provider di servizi condivisi NegoExts, quando il metodo di autenticazione ha esito negativo per qualsiasi motivo, verrà visualizzato o registrato un messaggio di errore di autenticazione. Non è possibile rinegoziare né metodi di autenticazione di fallback.

Percorso:%windir%\Windows\System32\negoexts.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nell'elenco **si applica a** all'inizio di questo argomento, ad eccezione di windows Server 2008 e Windows Vista.

### <a name="BKMK_PKU2USSP"></a>PKU2U Security Support Provider
Il protocollo PKU2U è stato introdotto e implementato come SSP in Windows 7 e Windows Server 2008 R2. Questo SSP consente l'autenticazione peer-to-peer, in particolare tramite la funzionalità di condivisione file e supporti denominata gruppo Home, introdotta in Windows 7. La funzionalità consente la condivisione tra computer che non sono membri di un dominio.

Percorso:%windir%\Windows\System32\pku2u.dll

Questo provider è incluso per impostazione predefinita nelle versioni indicate nell'elenco **si applica a** all'inizio di questo argomento, ad eccezione di windows Server 2008 e Windows Vista.

**Risorse aggiuntive per il protocollo PKU2U e il provider di servizi condivisi PKU2U**

-   [Introduzione all'integrazione di identità online](https://technet.microsoft.com/library/dd560662(v=ws.10).aspx)

## <a name="BKMK_SecuritySupportProviderSelection"></a>Selezione del provider di supporto per la sicurezza
Il servizio SSPI di Windows può utilizzare i protocolli supportati tramite i provider di supporto della sicurezza installati. Tuttavia, poiché non tutti i sistemi operativi supportano gli stessi pacchetti SSP di un determinato computer che esegue Windows Server, i client e i server devono negoziare per l'utilizzo di un protocollo supportato. Windows Server preferisce che i computer client e le applicazioni usino il protocollo Kerberos, un protocollo avanzato basato su standard, quando possibile, ma il sistema operativo continua a consentire i computer client e le applicazioni client che non supportano Kerberos protocollo per l'autenticazione.

Prima di poter eseguire l'autenticazione, i due computer che comunicano devono accettare il protocollo che entrambi possono supportare. Affinché qualsiasi protocollo possa essere utilizzato tramite SSPI, ogni computer deve disporre del provider di servizi condivisi appropriato. Ad esempio, affinché un computer client e un server usino il protocollo di autenticazione Kerberos, devono entrambi supportare Kerberos V5. Windows Server utilizza la funzione **EnumerateSecurityPackages** per identificare quali SSP sono supportati in un computer e quali sono le funzionalità di tali SSP.

La selezione di un protocollo di autenticazione può essere gestita in uno dei due modi seguenti:

1.  [Protocollo di autenticazione singola](#BKMK_SingleAuth)

2.  [Opzione Negotiate](#BKMK_Negotiate)

### <a name="BKMK_SingleAuth"></a>Protocollo di autenticazione singola
Quando nel server viene specificato un singolo protocollo accettabile, il computer client deve supportare il protocollo specificato oppure la comunicazione ha esito negativo. Quando viene specificato un solo protocollo accettabile, lo scambio di autenticazione viene eseguita come indicato di seguito:

1.  Il computer client richiede l'accesso a un servizio.

2.  Il server risponde alla richiesta e specifica il protocollo che verrà utilizzato.

3.  Il computer client esamina il contenuto della risposta e controlla per determinare se supporta il protocollo specificato. Se il computer client supporta il protocollo specificato, l'autenticazione continua. Se il computer client non supporta il protocollo, l'autenticazione ha esito negativo, indipendentemente dal fatto che il computer client sia autorizzato ad accedere alla risorsa.

### <a name="BKMK_Negotiate"></a>Opzione Negotiate
L'opzione Negotiate può essere usata per consentire al client e al server di provare a trovare un protocollo accettabile. Questa operazione si basa sul meccanismo di negoziazione API (SPNEGO) semplice e protetto. Quando l'autenticazione inizia con l'opzione per negoziare un protocollo di autenticazione, lo scambio SPNEGO viene eseguita come indicato di seguito:

1.  Il computer client richiede l'accesso a un servizio.

2.  Il server risponde con un elenco di protocolli di autenticazione che può supportare e una richiesta di autenticazione o una risposta, in base al protocollo che è la prima scelta. Ad esempio, il server potrebbe elencare il protocollo Kerberos e NTLM e inviare una risposta di autenticazione Kerberos.

3.  Il computer client esamina il contenuto della risposta e controlla per determinare se supporta uno dei protocolli specificati.

    -   Se il computer client supporta il protocollo preferito, l'autenticazione continua.

    -   Se il computer client non supporta il protocollo preferito, ma supporta uno degli altri protocolli elencati dal server, il computer client consente al server di individuare il protocollo di autenticazione supportato e di procedere con l'autenticazione.

    -   Se il computer client non supporta nessuno dei protocolli elencati, lo scambio di autenticazione ha esito negativo.

## <a name="see-also"></a>Vedi anche
[Architettura di Autenticazione di Windows](https://technet.microsoft.com/library/dn169024(v=ws.10).aspx)


