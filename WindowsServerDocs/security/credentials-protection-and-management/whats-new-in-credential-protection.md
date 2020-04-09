---
title: Novità della protezione delle credenziali
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-credential-protection
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f297
author: gitmichiko
ms.author: michikos
manager: dongill
ms.date: 03/06/2017
ms.openlocfilehash: 35097cee243239735995a00cec7a6fd3936c62a8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857044"
---
# <a name="whats-new-in-credential-protection"></a>Novità della protezione delle credenziali

## <a name="credential-guard-for-signed-in-user"></a>Credential Guard per l'utente connesso

A partire da Windows 10, versione 1507, Kerberos e NTLM utilizzano la sicurezza basata sulla virtualizzazione per proteggere Kerberos & i segreti NTLM della sessione di accesso dell'utente connesso. 

A partire da Windows 10, versione 1511, gestione credenziali usa la sicurezza basata sulla virtualizzazione per proteggere le credenziali salvate del tipo di credenziale di dominio. Le credenziali di accesso e le credenziali di dominio salvate non verranno passate a un host remoto tramite Desktop remoto. Credential Guard può essere abilitato senza blocco UEFI.

A partire da Windows 10, versione 1607, la modalità utente isolato è inclusa in Hyper-V in modo che non venga più installata separatamente per la distribuzione di Credential Guard.

[Altre informazioni su Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>Remote Credential Guard per l'utente che ha eseguito l'accesso

A partire da Windows 10, versione 1607, Remote Credential Guard protegge le credenziali dell'utente connesso quando si usa Desktop remoto proteggendo i segreti Kerberos e NTLM nel dispositivo client. Affinché l'host remoto valuti le risorse di rete come l'utente, le richieste di autenticazione richiedono che il dispositivo client usi i segreti.

A partire da Windows 10, versione 1703, Remote Credential Guard protegge le credenziali utente fornite quando si usa Desktop remoto.

[Altre informazioni su Remote Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Protezioni di dominio

Per la protezione del dominio è necessario un Active Directory dominio.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Supporto dei dispositivi aggiunti a un dominio per l'autenticazione con chiave pubblica

A partire da Windows 10 versione 1507 e Windows Server 2016, se un dispositivo aggiunto a un dominio è in grado di registrare la chiave pubblica associata con un controller di dominio di Windows Server 2016 (DC), il dispositivo può eseguire l'autenticazione con la chiave pubblica usando l'autenticazione Kerberos PKINIT in un controller di dominio di Windows Server 2016.

A partire da Windows Server 2016, KDC supporta l'autenticazione tramite l'attendibilità della chiave Kerberos.  

[Altre informazioni sul supporto della chiave pubblica per i dispositivi aggiunti a un dominio & attendibilità della chiave Kerberos](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Supporto dell'estensione per l'aggiornamento di PKINIT

A partire da Windows 10, versione 1507 e Windows Server 2016, i client Kerberos proveranno a usare l'estensione PKInit per l'accesso basato su chiave pubblica. 

A partire da Windows Server 2016, KDC può supportare l'estensione di aggiornamento PKInit.  Per impostazione predefinita, KDC non offrirà l'estensione di aggiornamento PKInit. 

[Altre informazioni sul supporto dell'estensione per l'aggiornamento di PKINIT](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>Scorrimento dei segreti NTLM dell'utente solo della chiave pubblica

A partire dal livello di funzionalità del dominio Windows Server 2016 (DFL), i controller di dominio possono supportare l'implementazione di una chiave pubblica solo per i segreti NTLM dell'utente. Questa funzionalità è unavailble in DFLs inferiori.

> [!WARNING] 
> L'aggiunta di un controller di dominio a un dominio con i segreti NTLM in sequenza abilitata prima che il controller di dominio sia stato aggiornato con almeno l'8 novembre, il servizio 2016 esegue il rischio di arresto anomalo del controller di dominio. 

Configurazione: per i nuovi domini, questa funzionalità è abilitata per impostazione predefinita. Per i domini esistenti, è necessario configurarlo nel centro di amministrazione Active Directory: 

1. Dal centro di amministrazione Active Directory fare clic con il pulsante destro del mouse sul dominio nel riquadro sinistro e selezionare **Proprietà**.

    ![Proprietà del dominio](../media/Credentials-Protection-And-Management/domain-properties.png)

2. Selezionare **Abilita sequenza dei segreti NTLM in scadenza durante l'accesso, per gli utenti che devono usare Microsoft Passport o smart card per l'accesso interattivo**.

    ![Registrazione dei segreti NTLM in scadenza](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Fare clic su **OK**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Consentire la rete NTLM quando l'utente è limitato a dispositivi appartenenti a un dominio specifico

A partire dal livello di funzionalità del dominio Windows Server 2016 (DFL), i controller di dominio possono supportare la rete NTLM quando un utente è limitato a dispositivi appartenenti a un dominio specifico. Questa funzionalità non è disponibile in DFLs inferiori.

Configurazione: nei criteri di autenticazione fare clic su **Consenti autenticazione di rete NTLM quando l'utente è limitato a dispositivi selezionati**. 

[Altre informazioni sui criteri di autenticazione](https://technet.microsoft.com/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
