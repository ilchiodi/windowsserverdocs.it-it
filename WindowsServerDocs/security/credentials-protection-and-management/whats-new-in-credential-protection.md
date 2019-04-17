---
title: "Novità di protezione delle credenziali"
description: Protezione di Windows Server
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
ms.openlocfilehash: 0556c606b987a69eae663b0196467f532d5a307a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-credential-protection"></a>Novità di protezione delle credenziali

## <a name="credential-guard-for-signed-in-user"></a>Credential Guard per l'utente connesso

A partire da Windows 10 versione 1507, Kerberos e NTLM usare protezione basata su virtualizzazione per proteggere i segreti Kerberos e NTLM della sessione di accesso utente connesso. 

A partire da Windows 10 versione 1511, Gestione credenziali Usa la sicurezza basata su virtualizzazione per proteggere le credenziali salvate del tipo di credenziali di dominio. Credenziali di accesso e le credenziali di dominio salvato non verrà passate a un host remoto tramite desktop remoto. È possibile attivare Credential Guard senza blocco UEFI.

A partire da Windows 10 versione 1607, modalità utente isolato è inclusa in Hyper-V in modo non viene installato separatamente per la distribuzione di Credential Guard.

[Altre informazioni su Credential Guard](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>Credential Guard remoto per l'utente connesso

A partire da Windows 10 versione 1607, Remote Credential Guard protegge le credenziali utente connesso quando si utilizza Desktop remoto per proteggere i segreti Kerberos e NTLM sul dispositivo client. Per l'host remoto valutare le risorse di rete dell'utente, le richieste di autenticazione richiedono il dispositivo client di utilizzare i segreti.

A partire da Windows 10, versione 1703, Remote Credential Guard protegge le credenziali utente fornito quando si utilizza Desktop remoto.

[Altre informazioni su Remote credential guard](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Protezioni di dominio

Le protezioni di dominio richiedono un dominio Active Directory.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Supporto dei dispositivi appartenenti a un dominio per l'autenticazione mediante chiave pubblica

A partire da Windows 10 versione 1507 e Windows Server 2016, se un dispositivo aggiunto al dominio è in grado di registrare la propria chiave pubblica associata con un controller di dominio (DC) di Windows Server 2016, quindi il dispositivo può eseguire l'autenticazione con la chiave pubblica con l'autenticazione Kerberos PKINIT a un controller di dominio di Windows Server 2016.

A partire da Windows Server 2016, il KDC supportano l'autenticazione utilizzando Kerberos chiave trust.  

[Ulteriori informazioni sul supporto di chiave pubblico per i dispositivi appartenenti a un dominio e trust di chiavi Kerberos](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Supporto delle estensioni di validità PKINIT

A partire da Windows 10 versione 1507 e Windows Server 2016, i client Kerberos tenterà l'estensione della validità PKInit per pubblico chiave in base ai punti di accesso. 

A partire da Windows Server 2016, i KDC possono supportare l'estensione della validità PKInit.  Per impostazione predefinita, KDC non offrirà l'estensione della validità PKInit. 

[Ulteriori informazioni sul supporto di estensione validità PKINIT](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>I segreti NTLM pubblico chiave solo dell'utente in sequenza

A partire da Windows Server 2016 livello funzionale del dominio (livello), i controller di dominio possono supportare i segreti NTLM pubblico chiave solo di un utente in sequenza. Questa funzionalità è originaria in DFLs inferiore.

> [!WARNING] 
> Aggiunta di un controller di dominio a un dominio con sequenza segreti NTLM attivati prima che il controller di dominio è stato aggiornato con almeno 8 novembre 2016 manutenzione viene eseguito il rischio di arresto anomalo di controller di dominio. 

Configurazione: Per i nuovi domini, questa funzionalità è abilitata per impostazione predefinita. Per i domini esistenti, deve essere configurato nel centro amministrativo di Active Directory: 

1. Dal centro di amministrazione di Active Directory, fare doppio clic su dominio nel riquadro di sinistra e selezionare **proprietà**.

    ![Proprietà dominio](../media/Credentials-Protection-And-Management/domain-properties.png)
    
2. Selezionare **abilitare in sequenza dei segreti NTLM in scadenza durante l'iscrizione, per gli utenti che sono necessari per usare Microsoft Passport o della smart card per l'accesso interattivo**.

    ![Segreti NTLM Autoroll che sta per scadere](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Fare clic su **OK**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Consentendo rete NTLM quando l'utente è limitato a specifici dispositivi aggiunti al dominio

A partire da Windows Server 2016 livello funzionale di dominio (livello), i controller di dominio può supportare rete consentendo NTLM quando un utente è limitata a specifici dispositivi aggiunti al dominio. Questa funzionalità non è disponibile in DFLs inferiore.

Configurazione: Criteri di autenticazione, fare clic su **seleziona l'autenticazione di rete consentire NTLM quando l'utente è limitata a dispositivi**. 

[Altre informazioni su criteri di autenticazione](https://technet.microsoft.com/en-us/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
