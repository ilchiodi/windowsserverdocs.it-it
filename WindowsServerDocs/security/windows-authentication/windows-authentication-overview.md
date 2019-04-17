---
title: Panoramica di autenticazione di Windows
description: Protezione di Windows Server
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
ms.openlocfilehash: c2ec55ed6b09628d1d80a24be766259980e84d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="windows-authentication-overview"></a>Panoramica di autenticazione di Windows

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento destinato ai professionisti IT Elenca le risorse di documentazione per le tecnologie di autenticazione e accesso di Windows che includono la valutazione del prodotto, le guide introduttive, le procedure, le guide alla progettazione e distribuzione, i riferimenti tecnici e i riferimenti al comando.

## <a name="feature-description"></a>Descrizione delle funzionalità
L'autenticazione è un processo di verifica dell'identità di un oggetto, un servizio o una persona. Quando si autentica un oggetto, l'obiettivo consiste nel verificare che l'oggetto sia autentico. Quando si autentica un servizio o una persona, l'obiettivo consiste nel verificare che le credenziali presentate siano autentiche.

In un contesto di rete, l'autenticazione è l'atto di dimostrare l'identità a un'applicazione di rete o una risorsa. In genere, l'identità viene dimostrata tramite un'operazione di crittografia che usa una chiave che nota solo all'utente - con crittografia a chiave pubblica - o una chiave condivisa. Sul lato server dello scambio di autenticazione confronta i dati firmati con una chiave crittografica nota per convalidare il tentativo di autenticazione.

Archiviare le chiavi di crittografia in una posizione centralizzata sicura rende il processo di autenticazione, scalabile e gestibile. Servizi di dominio Active Directory è consigliata e tecnologia predefinita per archiviare le informazioni di identità \ (incluse le chiavi crittografiche che sono credentials\ dell'utente). Active Directory è necessario per impostazione predefinita NTLM e Kerberos implementazioni.

Intervallo di tecniche di autenticazione da un semplice accesso, che identifica l'utente in base a un elemento che solo l'utente sa - simile a una password, per i meccanismi di sicurezza più potenti che usano un elemento che l'utente ha - come token, certificati di chiave pubblica e la biometria. In un ambiente aziendale, servizi o gli utenti possono accedere a più applicazioni o risorse su molti tipi di server all'interno di un'unica posizione o in diverse posizioni. Per questi motivi, l'autenticazione deve supportare ambienti per altre piattaforme e per altri sistemi operativi Windows.

Il sistema operativo Windows implementa un set predefinito di protocolli di autenticazione, tra cui Kerberos, NTLM, Transport Layer Security\/Secure Sockets Layer \(TLS\/SSL\) e Digest, come parte di un'architettura estensibile. Alcuni protocolli sono inoltre combinati in pacchetti di autenticazione, ad esempio Negotiate e Credential Security Support Provider. Questi protocolli e pacchetti consentono l'autenticazione di utenti, computer e servizi. il processo di autenticazione, a sua volta, consente a utenti e servizi autorizzati di accedere alle risorse in modo sicuro.

Per ulteriori informazioni sull'inclusione di autenticazione di Windows

-   [Concetti di autenticazione di Windows](windows-authentication-concepts.md)

-   [Scenari di accesso di Windows](windows-logon-scenarios.md)

-   [Architettura dell'autenticazione di Windows](windows-authentication-architecture.md)

-   [Architettura di interfaccia del Provider supporto sicurezza](security-support-provider-interface-architecture.md)

-   [Processi di credenziali in autenticazione di Windows](credentials-processes-in-windows-authentication.md)

-   [Impostazioni di criteri di gruppo usate nell'autenticazione di Windows](group-policy-settings-used-in-windows-authentication.md)

Vedere il [Panoramica tecnica di autenticazione di Windows](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Applicazioni pratiche
Autenticazione di Windows viene utilizzato per verificare che le informazioni provengano da una fonte attendibile, sia da un oggetto utente o computer, ad esempio un altro computer. Windows offre diversi metodi per raggiungere questo obiettivo, come descritto di seguito.

|A...|Funzionalità|Descrizione|
|----|------|--------|
|Autenticazione nell'ambito di un dominio Active Directory|Kerberos|Microsoft Windows&nbsp;sistemi operativi Server implementano il protocollo di autenticazione Kerberos versione 5 e le estensioni per l'autenticazione mediante chiave pubblica. Il client di autenticazione Kerberos viene implementato come security support provider \(SSP\) e sono accessibili tramite l'interfaccia del Provider supporto sicurezza \(SSPI\). Autenticazione iniziale degli utenti è integrato con l'architettura single Winlogon accesso-on. \(KDC\) il centro distribuzione chiavi Kerberos è integrato con altri servizi di sicurezza di Windows Server in esecuzione sul controller di dominio. Il KDC utilizza database del servizio directory Active Directory del dominio come database degli account di sicurezza. Active Directory è necessario per le implementazioni Kerberos predefinite.<br /><br />Per altre risorse, vedere [panoramica dell'autenticazione Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Autenticazione sicura sul web|TLS\/SSL come implementato nel Security Support Provider Schannel|Il \(TLS\) Transport Layer Security protocollo versioni 1.0, 1.1 e 1.2, il protocollo Secure Sockets Layer \(SSL\) versione del protocollo versioni 2.0 e 3.0, Datagram Transport Layer Security 1.0 e il protocollo Private Communications Transport \(PCT\) versione 1.0 sono basate sulla crittografia a chiave pubblica. La suite di protocolli Secure Channel \(Schannel\) provider autenticazione fornisce tali protocolli. Tutti i protocolli Schannel usano un modello client e server.<br /><br />Per altre risorse, vedere [TLS / SSL & #40; SSP Schannel & #41; Panoramica di](../tls/tls-ssl-schannel-ssp-overview.md).|
|L'autenticazione a un servizio web o un'applicazione|Autenticazione integrata di Windows<br /><br />Autenticazione del digest|For additional resources, see [Integrated Windows Authentication](https://technet.microsoft.com/library/cc758557(v=WS.10.aspx and [Digest Authentication](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx), and [Advanced Digest Authentication](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|Autenticazione in applicazioni legacy|NTLM|NTLM è un'aggiunta di autenticazione protocol.In stile challenge\-risposta di autenticazione, il protocollo NTLM gestisce la sicurezza della sessione, in particolare integrità e riservatezza tramite firma e sigillo in NTLM funzioni.<br /><br />Per altre risorse, vedere [Panoramica di NTLM](../kerberos/ntlm-overview.md).|
|Autenticazione a più fattori di utilizzo|Supporto di smart card<br /><br />Supporto biometria|Le smart card sono un modo portatile e resistente tamper\ per fornire soluzioni per la sicurezza per attività come l'autenticazione client, l'accesso ai domini, firma del codice e protezione e\ di posta elettronica.<br /><br />La biometria si basa sulla misurazione di una caratteristica fisica non mutevole di una persona per identificare la persona. Le impronte digitali sono una delle caratteristiche biometriche usate più di frequente, con milioni di dispositivi biometrici impronte digitali incorporati in personal computer e periferiche.<br /><br />Per altre risorse, vedere [documentazione tecnica su Smart Card](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Forniscono la gestione locale, archiviazione e riutilizzo delle credenziali|Gestione credenziali<br /><br />Autorità di protezione locale<br /><br />Password|Gestione delle credenziali in Windows assicura che le credenziali sono archiviate in modo sicuro. Le credenziali vengono raccolte sul Desktop sicuro \ (per access\ locale o di dominio), attraverso App o i siti Web in modo che le credenziali corrette vengono presentate ogni volta che si accede a una risorsa.<br /><br />
|Estendere la protezione di autenticazione moderna ai sistemi legacy|Protezione estesa per l'autenticazione|Questa funzionalità migliora la protezione e la gestione delle credenziali per l'autenticazione di connessioni di rete utilizzando \(IWA\) autenticazione integrata di Windows.|

## <a name="software-requirements"></a>Requisiti software
Autenticazione di Windows è progettato per essere compatibile con le versioni precedenti del sistema operativo Windows. Tuttavia, alcuni miglioramenti introdotti in ogni versione non sono necessariamente applicabili alle versioni precedenti. Fare riferimento alla documentazione sulle funzionalità specifiche per ulteriori informazioni.

## <a name="server-manager-information"></a>Informazioni su Server Manager
Molte funzionalità di autenticazione può essere configurata tramite criteri di gruppo, che può essere installato tramite Server Manager. La funzionalità di Windows Biometric Framework viene installata tramite Server Manager. Altri ruoli server che dipendono da metodi di autenticazione, ad esempio \(IIS\) Server Web e servizi di dominio Active Directory, possono essere installati tramite Server Manager.

## <a name="related-resources"></a>Risorse correlate

|Tecnologie di autenticazione|Risorse|
|----------------|-------|
|Autenticazione di Windows|[Panoramica tecnica di autenticazione di Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Include argomenti che illustrano le differenze tra le versioni, i concetti di autenticazione generale, gli scenari di accesso, le architetture per le versioni supportate e le impostazioni applicabili.|
|Kerberos|[Panoramica dell'autenticazione Kerberos](../kerberos/kerberos-authentication-overview.md)<br /><br />[Panoramica della delega vincolata Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Kerberos Authentication Technical Reference](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003\)<br /><br />[Kerberos Survival Guide](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(TechNet Wiki\)|
|TLS\/SSL e DTLS \ (Schannel security support provider)|[TLS / SSL & #40; SSP Schannel & #41; Panoramica](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Riferimento tecnico Provider supporto sicurezza Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Autenticazione del digest|[Digest Authentication Technical Reference](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003\)|
|NTLM|[Panoramica di NTLM](../kerberos/ntlm-overview.md)<br />Contiene collegamenti a risorse recenti e precedenti.|
|PKU2U|[Introduzione a PKU2U in Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Smart Card|[Documentazione tecnica su Smart Card](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|Credenziali|[Gestione e protezione delle credenziali](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contiene collegamenti a risorse recenti e precedenti.<br /><br />[Panoramica delle password](../kerberos/passwords-overview.md)<br />Contiene collegamenti a risorse recenti e precedenti.|


