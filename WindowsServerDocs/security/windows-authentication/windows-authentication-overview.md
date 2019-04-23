---
title: Panoramica di Autenticazione di Windows
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485a0774-0785-457f-a964-0e9403c12bb1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2021ccf1e0015cc910f43966f9400e6bd56a230c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870972"
---
# <a name="windows-authentication-overview"></a>Panoramica di Autenticazione di Windows

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento, destinato ai professionisti IT, include l'elenco delle risorse della documentazione relative alle tecnologie di autenticazione e accesso di Windows che includono la valutazione del prodotto, le guide introduttive, le procedure, le guide alla progettazione e alla distribuzione, i riferimenti tecnici e i riferimenti per i comandi.

## <a name="feature-description"></a>Descrizione delle caratteristiche
L'autenticazione è un processo che consente di verificare l'identità di un oggetto, di un servizio o di una persona. Quando si autentica un oggetto, lo scopo è verificare che tale oggetto sia autentico. Quando si autentica un servizio o una persona, l'obiettivo è quello di verificare che le credenziali presentate siano autentiche.

In un contesto di rete, l'autorizzazione consiste nel dimostrare l'identità a un'applicazione o risorsa di rete. In genere, l'identità viene dimostrata tramite un'operazione di crittografia che usa una chiave che nota solo all'utente, come con crittografia a chiave pubblica - o una chiave condivisa. Il lato server dello scambio di autenticazione confronta i dati firmati con una chiave crittografica nota per convalidare il tentativo di autenticazione.

L'archiviazione delle chiavi crittografiche in una posizione centralizzata sicura rende l'autenticazione un processo scalabile e gestibile. Servizi di dominio Active Directory è consigliata e la tecnologia predefinita per archiviare le informazioni di identità \(tra cui le chiavi crittografiche che sono le credenziali dell'utente\). Active Directory è necessario per le implementazioni NTLM e Kerberos predefinite.

Le tecniche di autenticazione compreso un semplice accesso, che identifica gli utenti in base a un elemento che è nota solo all'utente, ad esempio una password, ai meccanismi di sicurezza più potenti che usa un elemento che l'utente ha - come i token, certificati di chiave pubblica, e dati biometrici. In un ambiente aziendale, i servizi o gli utenti possono accedere a più applicazioni o risorse su molti tipi di server nell'ambito di un solo percorso o di più percorsi. Per tali motivi, l'autenticazione deve supportare ambienti per altre piattaforme e per altri sistemi operativi Windows.

Il sistema operativo Windows implementa un set predefinito di protocolli di autenticazione, tra cui Kerberos, NTLM, Transport Layer Security\/Secure Sockets Layer \(TLS\/SSL\)e Digest, come parte di un architettura estendibile. Alcuni protocolli sono inoltre combinati in pacchetti di autenticazione, ad esempio Negotiate e CredSSP (Credential Security Support Provider). Questi protocolli e pacchetti consentono l'autenticazione di utenti, computer e servizi. Il processo di autenticazione, a sua volta, consente agli utenti e ai servizi autorizzati di accedere alle risorse in modo sicuro.

Per altre informazioni sull'autenticazione di Windows, tra cui

-   [Concetti di autenticazione di Windows](windows-authentication-concepts.md)

-   [Scenari di accesso di Windows](windows-logon-scenarios.md)

-   [Architettura di autenticazione di Windows](windows-authentication-architecture.md)

-   [Architettura dell'interfaccia Provider supporto sicurezza](security-support-provider-interface-architecture.md)

-   [Processi di credenziali in autenticazione di Windows](credentials-processes-in-windows-authentication.md)

-   [Impostazioni di criteri di gruppo usate nell'autenticazione di Windows](group-policy-settings-used-in-windows-authentication.md)

vedere le [Panoramica tecnica di autenticazione di Windows](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Applicazioni pratiche
L'autenticazione di Windows viene utilizzata per verificare che le informazioni provengano da un'origine attendibile, indipendentemente che si tratti di una persona o di un oggetto computer, ad esempio un altro computer. In Windows sono disponibili diversi metodi per raggiungere questo obiettivo, come descritto di seguito.

|A...|Funzionalità|Descrizione|
|----|------|--------|
|Autenticazione nell'ambito di un dominio di Active Directory|Kerberos|Il Windows Microsoft&nbsp;i sistemi operativi Server implementano il protocollo Kerberos versione 5 e le estensioni per l'autenticazione con chiave pubblica. Il client di autenticazione Kerberos viene implementato come un provider supporto sicurezza \(SSP\) ed è possibile accedervi tramite l'interfaccia SSPI \(SSPI\). Autenticazione iniziale degli utenti è integrato con il single sign-Winlogon\-sull'architettura. Il centro distribuzione chiavi Kerberos \(KDC\) è integrato con altri servizi di sicurezza di Windows Server in esecuzione nel controller di dominio. Il KDC utilizza database del servizio del dominio Active Directory come database degli account di sicurezza. Active Directory è necessario per le implementazioni Kerberos predefinite.<br /><br />Per altre risorse, vedere [Panoramica dell'autenticazione Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Autenticazione sicura sul Web|TLS\/SSL come implementato nel Provider supporto sicurezza Schannel|Il protocollo Transport Layer Security \(TLS\) le versioni 1.0, 1.1 e 1.2, Secure Sockets Layer del protocollo \(SSL\) protocollo, la versione di protocollo versioni 2.0 e 3.0, Datagram Transport Layer Security 1.0 e privato Trasporto delle comunicazioni \(PCT\) protocollo, la versione 1.0, sono basati sulla crittografia a chiave pubblica. Secure Channel \(Schannel\) suite di protocolli di autenticazione provider fornisce tali protocolli. Tutti i protocolli Schannel utilizzano un modello client e server.<br /><br />Per altre risorse, vedere [TLS / SSL &#40;SSP Schannel&#41; Panoramica](../tls/tls-ssl-schannel-ssp-overview.md).|
|Autenticazione in un servizio Web o un'applicazione|Autenticazione integrata di Windows<br /><br />Autenticazione del digest|Per altre risorse, vedere gli argomenti su [autenticazione integrata di Windows](https://technet.microsoft.com/library/cc758557(v=WS.10).aspx), [autenticazione del digest](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx) e [autenticazione avanzata del digest](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|Autenticazione in applicazioni legacy|NTLM|NTLM è una sfida\-protocollo di autenticazione di stile di risposta. Oltre all'autenticazione, il protocollo NTLM, facoltativamente, fornisce sicurezza della sessione, in particolare l'integrità e riservatezza tramite firma e sigillo in NTLM funzioni.<br /><br />Per altre risorse, vedere [Panoramica di NTLM](../kerberos/ntlm-overview.md).|
|Utilizzare l'autenticazione a più fattori|Supporto di smart card<br /><br />Supporto biometria|Le smart card sono una manomissione\-portatile e resistente alle consentono di fornire soluzioni di sicurezza per attività quali l'autenticazione client, l'accesso ai domini, la firma del codice e la protezione e\-posta elettronica.<br /><br />La biometria si basa sulla misurazione di una caratteristica fisica non mutevole di una persona per identificarla in modo univoco. Le impronte digitali sono una delle caratteristiche biometriche utilizzate con maggiore frequenza, con milioni di dispositivi di biometria delle impronte digitali incorporati in personal computer e periferiche.<br /><br />Per altre risorse, vedere [tecnica delle Smart Card](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Consentire il riutilizzo, l'archiviazione e la gestione locale delle credenziali|Gestione di credenziali<br /><br />Autorità di sicurezza locale<br /><br />Password|La gestione delle credenziali in Windows assicura che le credenziali vengano archiviate in tutta sicurezza. Le credenziali vengono raccolte sul Desktop protetto \(per l'accesso locale o di dominio\), attraverso App o siti Web in modo che le credenziali corrette vengono presentate ogni volta che si accede a una risorsa.<br /><br />
|Estendere la protezione per l'autenticazione moderna ai sistemi legacy|Protezione estesa per l'autenticazione|Questa funzionalità migliora la protezione e la gestione delle credenziali per l'autenticazione di connessioni di rete usando l'autenticazione Windows integrata \(IWA\).|

## <a name="software-requirements"></a>Requisiti software
Autenticazione di Windows è compatibile con le versioni precedenti del sistema operativo Windows. Tuttavia, alcuni miglioramenti introdotti in ogni release non sono necessariamente applicabili alle versioni precedenti. Per altre informazioni, fare riferimento alla documentazione relativa alle funzionalità specifiche.

## <a name="server-manager-information"></a>Informazioni su Server Manager
È possibile configurare numerose funzionalità di autenticazione utilizzando la funzionalità Criteri di gruppo che può essere installata tramite Server Manager. La funzionalità Windows Biometric Framework viene installata tramite Server Manager. Altri ruoli del server che dipendono da metodi di autenticazione, ad esempio Server Web \(IIS\) e servizi di dominio di Active Directory, può anche essere installato tramite Server Manager.

## <a name="related-resources"></a>Risorse correlate

|Tecnologie di autenticazione|Risorse|
|----------------|-------|
|Autenticazione di Windows|[Panoramica tecnica di autenticazione di Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Sono inclusi argomenti che le differenze tra le versioni, i concetti di autenticazione generici, scenari di accesso, le architetture per le versioni supportate e le impostazioni applicabili.|
|Kerberos|[Panoramica dell'autenticazione Kerberos](../kerberos/kerberos-authentication-overview.md)<br /><br />[Panoramica della delega vincolata Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Riferimento tecnico sull'autenticazione Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<br /><br />[Guida di sopravvivenza per Kerberos](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(TechNet Wiki\)|
|TLS\/SSL e DTLS \(security support provider Schannel\)|[TLS / SSL &#40;SSP Schannel&#41; Panoramica](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Riferimento tecnico Provider supporto sicurezza Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Autenticazione del digest|[Riferimento tecnico sull'autenticazione digest](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[Panoramica di NTLM](../kerberos/ntlm-overview.md)<br />Contiene collegamenti a risorse correnti e precedenti|
|PKU2U|[Introduzione a PKU2U in Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Smart card|[Documentazione sulle Smart Card](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|Credenziali|[Gestione e protezione delle credenziali](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contiene collegamenti a risorse correnti e precedenti<br /><br />[Panoramica delle password](../kerberos/passwords-overview.md)<br />Contiene collegamenti a risorse correnti e precedenti|


