---
title: Architettura dell'autenticazione di Windows
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07c9d6bb-9b03-407d-89b6-97c7551b256b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fd6e2cbc233a91b62e3c8c9caf1154f91c3863ac
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="windows-authentication-architecture"></a>Architettura dell'autenticazione di Windows

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questa panoramica per i professionisti IT illustra lo schema dell'architettura di base per l'autenticazione di Windows.

L'autenticazione è il processo con cui il sistema convalida accesso o informazioni di accesso dell'utente. Nome e la password dell'utente vengono confrontati con un elenco autorizzato e se il sistema rileva una corrispondenza, viene concesso l'accesso a quanto specificato nell'elenco di autorizzazioni per tale utente.

Come parte di un'architettura estensibile, i sistemi operativi Windows Server implementano un set predefinito di autenticazione security support provider, che includono Negotiate, il protocollo Kerberos, NTLM, Schannel (canale sicuro) e Digest. I protocolli utilizzati da questi provider consentono l'autenticazione di utenti, computer e servizi e il processo di autenticazione consente a utenti e servizi autorizzati di accedere alle risorse in modo sicuro.

In Windows Server, le applicazioni autenticano gli utenti tramite SSPI per astratta chiamate per l'autenticazione. Di conseguenza, gli sviluppatori non è necessario comprendere la complessità dei protocolli di autenticazione di specifico o integrare le applicazioni di protocolli di autenticazione.

Sistemi operativi Windows Server includono un set di componenti di sicurezza che costituiscono il modello di sicurezza di Windows. Questi componenti assicurarsi che le applicazioni non possono accedere alle risorse senza autenticazione e autorizzazione. Le sezioni seguenti descrivono gli elementi dell'architettura di autenticazione.

### <a name="local-security-authority"></a>Autorità di protezione locale
L'autorità di sicurezza locale (LSA) è un sottosistema protetto che autentica ed effettua l'accesso agli utenti di computer locale. Inoltre, LSA gestisce le informazioni su tutti gli aspetti di sicurezza locale in un computer (questi aspetti sono noti come criterio di protezione locale). Fornisce inoltre diversi servizi per la conversione tra nomi e gli identificatori di sicurezza (SID).

Il sottosistema di sicurezza tiene traccia dei criteri di sicurezza e gli account che si trovano in un sistema. Nel caso di un dominio controller, questi criteri e gli account sono quelli che sono valide per il dominio in cui si trova il controller di dominio. Questi criteri e gli account vengono archiviati in Active Directory. Il sottosistema LSA fornisce servizi per convalidare l'accesso agli oggetti, controllo dei diritti utente e la generazione di messaggi di controllo.

### <a name="security-support-provider-interface"></a>Security Support Provider Interface
Security Support Provider Interface (SSPI) è l'API che ottiene i servizi di sicurezza integrata per l'autenticazione, integrità dei messaggi, riservatezza dei messaggi e sicurezza qualità del servizio per qualsiasi protocollo di applicazione distribuita.

SSPI è l'implementazione di API servizio di protezione generica (GSSAPI). SSPI fornisce un meccanismo mediante il quale è possibile che un'applicazione distribuita chiamare uno dei diversi provider di protezione per ottenere una connessione autenticata insaputa dei dettagli del protocollo di sicurezza.

## <a name="see-also"></a>Vedere anche

-   [Architettura di interfaccia del Provider supporto sicurezza](security-support-provider-interface-architecture.md)

-   [Processi di credenziali in autenticazione di Windows](credentials-processes-in-windows-authentication.md)

-   [Panoramica tecnica di autenticazione di Windows](https://technet.microsoft.com/library/dn169029.aspx)


