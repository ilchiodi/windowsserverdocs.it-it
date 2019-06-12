---
title: Nuove funzionalità di protezione delle credenziali
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f297
author: gitmichiko
ms.author: michikos
manager: dongill
ms.date: 03/06/2017
ms.openlocfilehash: 475b6a0b24b811008ee213c1604d98d9aa9eb092
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447030"
---
# <a name="whats-new-in-credential-protection"></a>Nuove funzionalità di protezione delle credenziali

## <a name="credential-guard-for-signed-in-user"></a>Credential Guard per utente connesso

A partire da Windows 10 versione 1507, Kerberos e NTLM usare sicurezza basata su virtualizzazione per proteggere i segreti di Kerberos e NTLM della sessione di accesso utente connesso. 

A partire da Windows 10 versione 1511, Gestione credenziali Usa la sicurezza basata sulla virtualizzazione per proteggere le credenziali salvate del tipo di credenziali di dominio. Le credenziali di firma e le credenziali di dominio salvata non verrà passate a un host remoto tramite desktop remoto. Credential Guard possono essere abilitate senza blocco UEFI.

A partire da Windows 10, versione 1607, modalità utente isolato è inclusa con Hyper-V in modo che non viene installata separatamente per la distribuzione di Credential Guard.

[Altre informazioni su Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>Credential Guard remoto per l'utente connesso

A partire da Windows 10, versione 1607, Credential Guard remoto consente di proteggere le credenziali dell'utente connesso quando si usa Desktop remoto per proteggere i segreti di Kerberos e NTLM nel dispositivo client. Per l'host remoto valutare le risorse di rete dell'utente, le richieste di autenticazione richiedono il dispositivo client di usare i segreti.

A partire da Windows 10 versione 1703, Credential Guard remoto consente di proteggere credenziali fornite dall'utente quando si usa Desktop remoto.

[Altre informazioni su Remote credential guard](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Protezioni di dominio

Protezioni di dominio richiedono un dominio Active Directory.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Supporto dei dispositivi aggiunti a un dominio per l'autenticazione tramite chiave pubblica

A partire da Windows 10 versione 1507 e Windows Server 2016, se un dispositivo aggiunto al dominio è in grado di registrare la propria chiave pubblica associata con un controller di dominio (DC) di Windows Server 2016, quindi il dispositivo può eseguire l'autenticazione con la chiave pubblica usando Kerberos PKINIT autenticazione a un controller di dominio di Windows Server 2016.

A partire da Windows Server 2016, KDC supportano l'autenticazione tramite trust chiavi Kerberos.  

[Altre informazioni sul supporto di chiavi pubblico per i dispositivi aggiunti a un dominio e attendibilità chiavi Kerberos](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Supporto delle estensioni Freshness PKINIT

A partire da Windows 10, versione 1507 e Windows Server 2016, i client Kerberos tenterà l'estensione freshness PKInit per pubblico chiave basata su punti di accesso. 

A partire da Windows Server 2016, i KDC può supportare l'estensione freshness PKInit.  Per impostazione predefinita, KDC non offrirà l'estensione freshness PKInit. 

[Altre informazioni sul supporto di estensione freshness PKINIT](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>In sequenza i segreti NTLM pubblico chiave solo dell'utente

A partire da livello funzionale del dominio Windows Server 2016 (funzionalità del dominio), i controller di dominio può supportare in sequenza i segreti NTLM pubblico chiave solo un utente. Questa funzionalità è originaria in DFLs inferiore.

> [!WARNING] 
> Aggiunta di un controller di dominio a un dominio con in sequenza i segreti NTLM attivati prima che il controller di dominio è stata aggiornata con almeno l'8 novembre 2016 per la manutenzione viene eseguito il rischio che il controller di dominio di un arresto anomalo. 

Configurazione: Per i nuovi domini, questa funzionalità è abilitata per impostazione predefinita. Per i domini esistenti, deve essere configurato nel centro amministrativo di Active Directory: 

1. Dal centro amministrativo di Active Directory, fare clic sul dominio nel riquadro sinistro e selezionare **proprietà**.

    ![Proprietà di dominio](../media/Credentials-Protection-And-Management/domain-properties.png)

2. Selezionare **abilitare in sequenza dei segreti NTLM in scadenza durante l'accesso, per gli utenti che hanno richiesto l'uso di Microsoft Passport o una smart card per l'accesso interattivo**.

    ![Segreti NTLM Autoroll in scadenza](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Fare clic su **OK**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Consente di rete NTLM quando l'utente è limitato ai dispositivi aggiunti a un dominio specifici

Partire da Windows Server 2016 livello funzionale del dominio (livello), i controller di dominio può supportare la rete che consente NTLM quando un utente è limitato a specifici dispositivi aggiunti a un dominio. Questa funzionalità non è disponibile in DFLs inferiore.

Configurazione: Nei criteri di autenticazione, fare clic su **autenticazione di rete consente NTLM quando l'utente è limitato ai dispositivi selezionati**. 

[Altre informazioni sui criteri di autenticazione](https://technet.microsoft.com/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
