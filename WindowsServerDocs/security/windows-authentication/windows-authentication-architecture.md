---
title: Architettura di Autenticazione di Windows
description: Sicurezza di Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855862"
---
# <a name="windows-authentication-architecture"></a>Architettura di Autenticazione di Windows

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento di panoramica per i professionisti IT illustra lo schema dell'architettura di base per l'autenticazione di Windows.

L'autenticazione è il processo mediante il quale il sistema convalida le informazioni di accesso o di accesso dell'utente. Nome e la password dell'utente vengono confrontate con un elenco autorizzato, e se il sistema rileva una corrispondenza, l'accesso viene concesso per l'extent specificato nell'elenco di autorizzazioni per tale utente.

Come parte di un'architettura estensibile, i sistemi operativi Windows Server implementano un set predefinito di autenticazione security support provider, che includono Negotiate, il protocollo Kerberos, NTLM, Schannel (canale sicuro) e Digest. I protocolli utilizzati da questi provider di abilitare l'autenticazione degli utenti, computer e servizi e il processo di autenticazione consente agli utenti autorizzati e servizi accedere alle risorse in modo sicuro.

In Windows Server, le applicazioni autenticano gli utenti usando l'interfaccia SSPI per astrarre le chiamate per l'autenticazione. Di conseguenza, gli sviluppatori non sono necessario comprendere le complessità dei protocolli di autenticazione specifica o compilare i protocolli di autenticazione nelle proprie applicazioni.

Sistemi operativi Windows Server includono un set di componenti di sicurezza che costituiscono il modello di sicurezza di Windows. Questi componenti assicurarsi che le applicazioni non è possibile ottenere l'accesso alle risorse senza autenticazione e autorizzazione. Le sezioni seguenti descrivono gli elementi dell'architettura di autenticazione.

### <a name="local-security-authority"></a>Autorità di sicurezza locale
L'autorità di sicurezza locale (LSA) è un sottosistema protetto che autentica e accesso degli utenti al computer locale. LSA gestisce inoltre informazioni su tutti gli aspetti di sicurezza locale in un computer (questi aspetti sono noti collettivamente come criterio di protezione locale). Fornisce anche diversi servizi per la conversione tra i nomi e ID di sicurezza (SID).

Il sottosistema di sicurezza tiene traccia di criteri di sicurezza e gli account che si trovano in un sistema informatico. Nel caso di un dominio controller, questi criteri e gli account sono quelli che sono attive per il dominio in cui si trova il controller di dominio. Questi criteri e gli account vengono archiviati in Active Directory. Il sottosistema LSA fornisce servizi per la convalida dell'accesso agli oggetti, controllo dei diritti utente e la generazione di messaggi di controllo.

### <a name="security-support-provider-interface"></a>Security Support Provider Interface
Security Support Provider Interface (SSPI) è l'API che ottiene i servizi di sicurezza integrata per l'autenticazione, l'integrità, riservatezza dei messaggi e sicurezza qualità del servizio per un qualsiasi protocollo di applicazione distribuita.

SSPI è l'implementazione del generico sicurezza del servizio API GSSAPI (). SSPI fornisce un meccanismo mediante il quale un'applicazione distribuita può chiamare uno dei diversi provider di sicurezza per ottenere una connessione autenticata senza conoscere i dettagli del protocollo di sicurezza.

## <a name="see-also"></a>Vedere anche

-   [Architettura dell'interfaccia Provider supporto sicurezza](security-support-provider-interface-architecture.md)

-   [Processi di credenziali in autenticazione di Windows](credentials-processes-in-windows-authentication.md)

-   [Panoramica tecnica di autenticazione di Windows](https://technet.microsoft.com/library/dn169029.aspx)


