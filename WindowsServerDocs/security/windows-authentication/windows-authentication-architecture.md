---
title: Architettura di Autenticazione di Windows
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 07c9d6bb-9b03-407d-89b6-97c7551b256b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: f2a2b9db60842ba7889116cf35163c579d9131d1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861724"
---
# <a name="windows-authentication-architecture"></a>Architettura di Autenticazione di Windows

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento di panoramica per i professionisti IT illustra lo schema architetturale di base per l'autenticazione di Windows.

L'autenticazione è il processo mediante il quale il sistema convalida le informazioni di accesso o accesso dell'utente. Il nome e la password di un utente vengono confrontati con un elenco autorizzato e se il sistema rileva una corrispondenza, viene concesso l'accesso all'extent specificato nell'elenco di autorizzazioni per tale utente.

Come parte di un'architettura estensibile, i sistemi operativi Windows Server implementano un set predefinito di provider di supporto per la sicurezza di autenticazione, tra cui negoziazione, protocollo Kerberos, NTLM, Schannel (canale protetto) e digest. I protocolli utilizzati da questi provider abilitano l'autenticazione di utenti, computer e servizi e il processo di autenticazione consente a utenti e servizi autorizzati di accedere alle risorse in modo sicuro.

In Windows Server, le applicazioni autenticano gli utenti usando SSPI per astrarre le chiamate per l'autenticazione. Pertanto, gli sviluppatori non devono comprendere le complessità dei protocolli di autenticazione specifici o creare protocolli di autenticazione nelle applicazioni.

I sistemi operativi Windows Server includono un set di componenti di sicurezza che costituiscono il modello di sicurezza di Windows. Questi componenti assicurano che le applicazioni non possano ottenere l'accesso alle risorse senza autenticazione e autorizzazione. Le sezioni seguenti descrivono gli elementi dell'architettura di autenticazione.

### <a name="local-security-authority"></a>Autorità di sicurezza locale
L'autorità di sicurezza locale (LSA) è un sottosistema protetto che esegue l'autenticazione e l'accesso degli utenti al computer locale. Inoltre, LSA mantiene le informazioni su tutti gli aspetti della sicurezza locale in un computer (questi aspetti sono collettivamente noti come criteri di sicurezza locali). Fornisce inoltre vari servizi per la conversione tra nomi e ID di sicurezza (SID).

Il sottosistema di sicurezza tiene traccia dei criteri di sicurezza e degli account che si trovano in un sistema di computer. Nel caso di un controller di dominio, questi criteri e account sono quelli attivi per il dominio in cui si trova il controller di dominio. Questi criteri e account vengono archiviati in Active Directory. Il sottosistema LSA fornisce servizi per la convalida dell'accesso a oggetti, il controllo dei diritti utente e la generazione di messaggi di controllo.

### <a name="security-support-provider-interface"></a>Interfaccia Security Support Provider
Security Support Provider Interface (SSPI) è l'API che ottiene servizi di sicurezza integrati per l'autenticazione, l'integrità dei messaggi, la privacy dei messaggi e la qualità del servizio di sicurezza per qualsiasi protocollo applicativo distribuito.

SSPI è l'implementazione dell'API del servizio di sicurezza generico (GSSAPI). SSPI fornisce un meccanismo mediante il quale un'applicazione distribuita può chiamare uno dei diversi provider di sicurezza per ottenere una connessione autenticata senza conoscere i dettagli del protocollo di sicurezza.

## <a name="see-also"></a>Vedere anche

-   [Architettura dell'interfaccia del provider supporto Sicurezza](security-support-provider-interface-architecture.md)

-   [Processi di credenziali in Autenticazione di Windows](credentials-processes-in-windows-authentication.md)

-   [Panoramica tecnica di Autenticazione di Windows](https://technet.microsoft.com/library/dn169029.aspx)


