---
title: Panoramica di Autenticazione di Windows
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 262a510e0b8484f3ee5cc28726f10857f92cbfd4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403286"
---
# <a name="windows-authentication-overview"></a>Panoramica di Autenticazione di Windows

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento, destinato ai professionisti IT, include l'elenco delle risorse della documentazione relative alle tecnologie di autenticazione e accesso di Windows che includono la valutazione del prodotto, le guide introduttive, le procedure, le guide alla progettazione e alla distribuzione, i riferimenti tecnici e i riferimenti per i comandi.

## <a name="feature-description"></a>Descrizione delle caratteristiche
L'autenticazione è un processo che consente di verificare l'identità di un oggetto, di un servizio o di una persona. Quando si autentica un oggetto, lo scopo è verificare che tale oggetto sia autentico. Quando si autentica un servizio o una persona, l'obiettivo è quello di verificare che le credenziali presentate siano autentiche.

In un contesto di rete, l'autorizzazione consiste nel dimostrare l'identità a un'applicazione o risorsa di rete. In genere, l'identità viene dimostrata da un'operazione di crittografia che utilizza una chiave solo l'utente sa come con la crittografia a chiave pubblica o una chiave condivisa. Il lato server dello scambio di autenticazione confronta i dati firmati con una chiave crittografica nota per convalidare il tentativo di autenticazione.

L'archiviazione delle chiavi crittografiche in una posizione centralizzata sicura rende l'autenticazione un processo scalabile e gestibile. Active Directory Domain Services è la tecnologia consigliata e predefinita per l'archiviazione delle informazioni di identità \(including le chiavi crittografiche che rappresentano le credenziali dell'utente @ no__t-1. Active Directory è necessario per le implementazioni NTLM e Kerberos predefinite.

Le tecniche di autenticazione variano da un semplice accesso, che identifica gli utenti in base a un elemento che solo l'utente conosce, ad esempio una password, a meccanismi di sicurezza più potenti che usano un elemento che l'utente ha come token, certificati di chiave pubblica e biometria. In un ambiente aziendale, i servizi o gli utenti possono accedere a più applicazioni o risorse su molti tipi di server nell'ambito di un solo percorso o di più percorsi. Per tali motivi, l'autenticazione deve supportare ambienti per altre piattaforme e per altri sistemi operativi Windows.

Il sistema operativo Windows implementa un set predefinito di protocolli di autenticazione, tra cui Kerberos, NTLM, Transport Layer Security @ no__t-0Secure Sockets Layer \(TLS @ no__t-2SSL @ no__t-3 e digest, come parte di un'architettura estendibile. Alcuni protocolli sono inoltre combinati in pacchetti di autenticazione, ad esempio Negotiate e CredSSP (Credential Security Support Provider). Questi protocolli e pacchetti consentono l'autenticazione di utenti, computer e servizi. Il processo di autenticazione, a sua volta, consente agli utenti e ai servizi autorizzati di accedere alle risorse in modo sicuro.

Per altre informazioni sull'autenticazione di Windows, tra cui

-   [Concetti su Autenticazione di Windows](windows-authentication-concepts.md)

-   [Scenari di accesso di Windows](windows-logon-scenarios.md)

-   [Architettura di Autenticazione di Windows](windows-authentication-architecture.md)

-   [Architettura dell'interfaccia del provider supporto Sicurezza](security-support-provider-interface-architecture.md)

-   [Processi di credenziali in Autenticazione di Windows](credentials-processes-in-windows-authentication.md)

-   [Impostazioni dei criteri di gruppo usate in Autenticazione di Windows](group-policy-settings-used-in-windows-authentication.md)

vedere [Panoramica tecnica sull'autenticazione di Windows](windows-authentication-technical-overview.md).

## <a name="practical-applications"></a>Applicazioni pratiche
L'autenticazione di Windows viene utilizzata per verificare che le informazioni provengano da un'origine attendibile, indipendentemente che si tratti di una persona o di un oggetto computer, ad esempio un altro computer. In Windows sono disponibili diversi metodi per raggiungere questo obiettivo, come descritto di seguito.

|A...|Funzionalità|Descrizione|
|----|------|--------|
|Autenticazione nell'ambito di un dominio di Active Directory|Kerberos|I sistemi operativi Microsoft Windows @ no__t-0Server implementano il protocollo di autenticazione Kerberos versione 5 e le estensioni per l'autenticazione con chiave pubblica. Il client di autenticazione Kerberos viene implementato come Security Support Provider \(SSP @ no__t-1 ed è possibile accedervi tramite l'interfaccia Security Support Provider \(SSPI @ no__t-3. L'autenticazione iniziale degli utenti è integrata con l'architettura di Winlogon Single Sign @ no__t-0on. Il Centro distribuzione chiavi Kerberos \(KDC @ no__t-1 è integrato con altri servizi di sicurezza di Windows Server in esecuzione sul controller di dominio. Il KDC utilizza il database del servizio directory Active Directory del dominio come database degli account di sicurezza. Active Directory è necessario per le implementazioni Kerberos predefinite.<br /><br />Per altre risorse, vedere [Panoramica dell'autenticazione Kerberos](../kerberos/kerberos-authentication-overview.md).|
|Autenticazione sicura sul Web|TLS @ no__t-0SSL come implementato nel Security Support Provider Schannel|Le versioni del protocollo Transport Layer Security \(TLS @ no__t-1 sono 1,0, 1,1 e 1,2, Secure Sockets Layer \(SSL @ no__t-3, versioni 2,0 e 3,0, datagramma Transport Layer Security protocollo versione 1,0 e trasporto di comunicazioni private il protocollo \(PCT @ no__t-5, versione 1,0, si basa sulla crittografia a chiave pubblica. Il canale sicuro \(Schannel @ no__t-1 provider Authentication Protocol Suite fornisce questi protocolli. Tutti i protocolli Schannel utilizzano un modello client e server.<br /><br />Per altre risorse, vedere [Panoramica SSP&#41; TLS &#40;-SSL Schannel](../tls/tls-ssl-schannel-ssp-overview.md).|
|Autenticazione in un servizio Web o un'applicazione|Autenticazione integrata di Windows<br /><br />Autenticazione del digest|Per altre risorse, vedere gli argomenti su [autenticazione integrata di Windows](https://technet.microsoft.com/library/cc758557(v=WS.10).aspx), [autenticazione del digest](https://technet.microsoft.com/library/cc738318(v=ws.10).aspx) e [autenticazione avanzata del digest](https://technet.microsoft.com/library/cc783131(v=ws.10).aspx).|
|Autenticazione in applicazioni legacy|NTLM|NTLM è un protocollo di autenticazione di tipo Challenge @ no__t-0response. Oltre all'autenticazione, il protocollo NTLM fornisce facoltativamente la sicurezza della sessione, in particolare l'integrità e la riservatezza dei messaggi tramite funzioni di firma e sealing in NTLM.<br /><br />Per altre risorse, vedere [Panoramica di NTLM](../kerberos/ntlm-overview.md).|
|Utilizzare l'autenticazione a più fattori|Supporto di smart card<br /><br />Supporto biometria|Le smart card sono un metodo manomissione @ no__t-0resistant e portabile per offrire soluzioni di sicurezza per attività come l'autenticazione client, l'accesso ai domini, la firma del codice e la protezione di e @ no__t-1mail.<br /><br />La biometria si basa sulla misurazione di una caratteristica fisica non mutevole di una persona per identificarla in modo univoco. Le impronte digitali sono una delle caratteristiche biometriche utilizzate con maggiore frequenza, con milioni di dispositivi di biometria delle impronte digitali incorporati in personal computer e periferiche.<br /><br />Per altre risorse, vedere [riferimento tecnico per Smart Card](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference). |
|Consentire il riutilizzo, l'archiviazione e la gestione locale delle credenziali|Gestione di credenziali<br /><br />Autorità di sicurezza locale<br /><br />Password|La gestione delle credenziali in Windows assicura che le credenziali vengano archiviate in tutta sicurezza. Le credenziali vengono raccolte sul desktop sicuro \(per locale o sul dominio di accesso @ no__t-1, attraverso app o siti Web in modo che le credenziali corrette vengano presentate ogni volta che si accede a una risorsa.<br /><br />
|Estendere la protezione per l'autenticazione moderna ai sistemi legacy|Protezione estesa per l'autenticazione|Questa funzionalità consente di migliorare la protezione e la gestione delle credenziali durante l'autenticazione delle connessioni di rete usando l'autenticazione integrata di Windows \(IWA @ no__t-1.|

## <a name="software-requirements"></a>Requisiti software
Autenticazione di Windows è compatibile con le versioni precedenti del sistema operativo Windows. Tuttavia, alcuni miglioramenti introdotti in ogni release non sono necessariamente applicabili alle versioni precedenti. Per altre informazioni, fare riferimento alla documentazione relativa alle funzionalità specifiche.

## <a name="server-manager-information"></a>Informazioni su Server Manager
È possibile configurare numerose funzionalità di autenticazione utilizzando la funzionalità Criteri di gruppo che può essere installata tramite Server Manager. La funzionalità Windows Biometric Framework viene installata tramite Server Manager. Gli altri ruoli del server che dipendono da metodi di autenticazione, ad esempio server Web \(IIS @ no__t-1 e Active Directory Domain Services, possono essere installati anche usando Server Manager.

## <a name="related-resources"></a>Risorse correlate

|Tecnologie di autenticazione|Risorse|
|----------------|-------|
|Autenticazione di Windows|[Panoramica tecnica di Autenticazione di Windows](../windows-authentication/windows-authentication-technical-overview.md)<br />Include argomenti che illustrano le differenze tra le versioni, i concetti generali di autenticazione, gli scenari di accesso, le architetture per le versioni supportate e le impostazioni applicabili.|
|Kerberos|[Panoramica dell'autenticazione Kerberos](../kerberos/kerberos-authentication-overview.md)<br /><br />[Panoramica della delega vincolata Kerberos](../kerberos/kerberos-constrained-delegation-overview.md)<br /><br />[Riferimento tecnico per l'autenticazione Kerberos](https://technet.microsoft.com/library/cc739058(v=ws.10).aspx)\(2003 @ no__t-2<br /><br />[Guida alla sopravvivenza di Kerberos](https://social.technet.microsoft.com/wiki/contents/articles/4209.kerberos-survival-guide.aspx) \(TechNet wiki @ no__t-2|
|TLS @ no__t-0SSL e DTLS \(Schannel Security Support Provider @ no__t-2|[Panoramica SSP&#41; TLS &#40;-SSL Schannel](../tls/tls-ssl-schannel-ssp-overview.md)<br /><br />[Documentazione tecnica su Security Support Provider Schannel](../tls/schannel-security-support-provider-technical-reference.md)|
|Autenticazione del digest|[Riferimento tecnico per l'autenticazione del Digest](https://technet.microsoft.com/library/cc782794(v=ws.10).aspx)\(2003 @ no__t-2|
|NTLM|[Panoramica di NTLM](../kerberos/ntlm-overview.md)<br />Contiene collegamenti alle risorse correnti e precedenti|
|PKU2U|[Introduzione a PKU2U in Windows](https://technet.microsoft.com/library/dd560634(v=ws.10).aspx)|
|Smart card|[Riferimento tecnico per Smart Card](https://technet.microsoft.com/itpro/windows/keep-secure/smart-card-windows-smart-card-technical-reference)<br /><br />
|Credenziali|[Gestione e protezione delle credenziali](../credentials-protection-and-management/credentials-protection-and-management.md)<br />Contiene collegamenti alle risorse correnti e precedenti<br /><br />[Panoramica delle password](../kerberos/passwords-overview.md)<br />Contiene collegamenti alle risorse correnti e precedenti|


