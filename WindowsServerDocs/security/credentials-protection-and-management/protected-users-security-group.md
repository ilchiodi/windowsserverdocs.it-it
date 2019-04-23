---
title: Gruppo di sicurezza Utenti protetti
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f296
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: bd6b53c0febdfb2d344136097a9654c981405568
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862362"
---
# <a name="protected-users-security-group"></a>Gruppo di sicurezza Utenti protetti

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento destinato al professionista IT viene descritto il gruppo di sicurezza Utenti protetti di Active Directory e viene spiegato come funziona. Questo gruppo è stato introdotto in Windows Server 2012 R2 controller di dominio.

## <a name="BKMK_ProtectedUsers"></a>Panoramica

Questo gruppo di sicurezza è progettato come parte di una strategia per gestire l'esposizione delle credenziali all'interno dell'organizzazione. Agli account dei membri di questo gruppo vengono applicati automaticamente protezioni non configurabili. Per impostazione predefinita, l'appartenenza al gruppo Utenti protetti è progettata per essere restrittiva e sicura in modo proattivo. L'unico metodo per modificare queste protezioni per gli account consiste nel rimuovere l'account dal gruppo di sicurezza.

> [!WARNING]
> Account per servizi e i computer non devono essere mai membri del gruppo utenti protetti. Questo gruppo sarebbe offre protezione incompleta comunque perché la password o il certificato è sempre disponibile nell'host. L'autenticazione avrà esito negativo con l'errore \"il nome utente o password non è corretta\" per qualsiasi servizio o un computer in cui viene aggiunto al gruppo utenti protetti.

Questo gruppo globale, ai domini attiva una protezione non configurabile sui dispositivi e computer host che eseguono Windows Server 2012 R2 e Windows 8.1 o versioni successive per gli utenti nei domini con controller di dominio primario che esegue Windows Server 2012 R2. Questo riduce notevolmente il footprint di memoria predefinita delle credenziali quando gli utenti effettuato l'accesso a computer con queste protezioni.

Per altre informazioni, vedere [modo in cui gli utenti protetti gruppo works](#BKMK_HowItWorks) in questo argomento.



## <a name="BKMK_Requirements"></a>Requisiti di gruppo utenti protetti
I requisiti per le protezioni di dispositivo per i membri del gruppo utenti protetti includono:

- Il gruppo di sicurezza globale Utenti protetti viene replicato in tutti i controller di dominio del dominio account.

- Windows 8.1 e Windows Server 2012 R2 aggiunto il supporto per impostazione predefinita. [Microsoft Security Advisory 2871997](https://technet.microsoft.com/library/security/2871997) aggiunge il supporto per Windows 7, Windows Server 2008 R2 e Windows Server 2012.

I requisiti per la protezione con controller di dominio per i membri del gruppo Utenti protetti includono:

- Gli utenti devono trovarsi in domini che sono Windows Server 2012 R2 o versione successiva livello funzionale del dominio.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Gruppo di sicurezza globale utenti protetti aggiunta ai domini di livello inferiore

Controller di dominio che eseguono un sistema operativo precedente a Windows Server 2012 R2 può supportare l'aggiunta di membri al nuovo gruppo di sicurezza utenti protetti. Ciò consente agli utenti di trarre vantaggio dalla funzionalità di protezione di dispositivi prima che il dominio viene aggiornato. 

> [!Note]
> I controller di dominio non supporterà la protezioni di dominio. 

Gruppo utenti protetti può essere creato da [trasferimento del ruolo di emulatore dominio primario (PDC) controller](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) a controller di dominio che esegue Windows Server 2012 R2. Dopo avere replicato l'oggetto gruppo in altri controller di dominio, il ruolo dell'emulatore PDC può essere ospitato in un controller di dominio che esegue una versione precedente di Windows Server.

### <a name="BKMK_ADgroup"></a>Proprietà del gruppo AD utenti protette

La seguente tabella specifica le proprietà del gruppo Utenti protetti.

|Attributo|Value|
|-------|-----|
|SID/RID noto|S-1-5-21-<domain>-525|
|Tipo|Dominio globale|
|Contenitore predefinito|CN=Utenti, DC=<domain>, DC=|
|Membri predefiniti|Nessuno|
|Membro predefinito di|Nessuno|
|Protetto da ADMINSDHOLDER?|No|
|È sicuro spostarlo fuori dal contenitore predefinito?|Yes|
|È sicuro delegare la gestione di questo gruppo ad amministratori non di servizio?|No|
|Diritti utente predefiniti|Nessun diritto utente predefinito|

## <a name="BKMK_HowItWorks"></a>Come funziona il gruppo utenti protetti
In questa sezione viene spiegato come funziona il gruppo Utenti protetti quando:

-   Firmato in un dispositivo Windows

-   Dominio dell'account utente è in un Windows Server 2012 R2 o versione successiva livello funzionale del dominio

### <a name="device-protections-for-signed-in-protected-users"></a>Protezioni di dispositivo per effettuato da utenti protetti
Quando l'utente connesso è membro del gruppo utenti protetti vengono applicate le misure di sicurezza seguenti:

-   Credenziali will delega (CredSSP) non cache credenziali in testo normale dell'utente, anche se il **Consenti delega credenziali predefinite** è abilitata l'impostazione di criteri di gruppo.

-   A partire da Windows 8.1 e Windows Server 2012 R2, Windows Digest non nella cache le credenziali dell'utente testo normale anche quando è abilitato il Digest di Windows.

> [!Note]
> Dopo aver installato [Microsoft Security Advisory 2871997](https://technet.microsoft.com/library/security/2871997) Digest di Windows continuerà alle credenziali nella cache fino a quando non è configurata la chiave del Registro di sistema. Vedere [Advisory Microsoft sulla sicurezza: Aggiornamento per migliorare la gestione e protezione delle credenziali: 13 maggio 2014](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) per le istruzioni.

-   NTLM non vengono memorizzati nella cache le credenziali in testo normale dell'utente o una funzione unidirezionale NT (NTOWF).

-   Kerberos non è più creerà le chiavi di DES o RC4. Inoltre non nella cache le credenziali in testo normale o chiavi a lungo termine dell'utente dopo che è stato acquisito il TGT iniziale.

-   Un sistema di verifica memorizzato nella cache non viene creato al momento dell'accesso o lo sblocco, in modo Accedi offline non sono più supportata.

Dopo che l'account utente viene aggiunto al gruppo utenti protetti, la protezione inizierà quando l'utente esegue l'accesso al dispositivo.

### <a name="domain-controller-protections-for-protected-users"></a>Protezioni di controller di dominio per utenti protetti
Account che sono membri del gruppo utenti protetti che eseguono l'autenticazione a un dominio di Windows Server 2012 R2 in grado di:

-   Usare l'autenticazione NTLM.

-   Usare i tipi di crittografia DES o RC4 nella preautenticazione Kerberos.

-   Usare la delega vincolata o non vincolata.

-   Rinnovare i ticket di concessione ticket di Kerberos oltre la durata iniziale di quattro ore.

Le impostazioni non configurabili relative alla scadenza dei ticket di concessione ticket vengono stabilite per ciascun account nel gruppo Utenti protetti. Normalmente, il controller di dominio imposta la durata e il rinnovo dei ticket di concessione ticket in base ai criteri di dominio **Durata massima ticket utente** e **Durata massima rinnovo ticket utente**. Per il gruppo Utenti protetti, viene impostato 600 minuti per i criteri di dominio.

Per altre informazioni, vedere [Come configurare gli account protetti](how-to-configure-protected-accounts.md).

## <a name="troubleshooting"></a>Risoluzione dei problemi
Sono disponibili due registri amministrativi per la risoluzione dei problemi relativi agli eventi correlati a Utenti protetti. Questi nuovi registri si trovano nel Visualizzatore eventi e sono disabilitati per impostazione predefinita. Sono disponibili nel percorso **Registri applicazioni e servizi\Microsoft\Windows\Microsoft\Autenticazione**.

|ID e registro eventi|Descrizione|
|----------|--------|
|104<br /><br />**ProtectedUser-Client**|Motivo: Il pacchetto di protezione nel client non contiene le credenziali.<br /><br />L'errore viene registrato nel computer client quando l'account è membro del gruppo di sicurezza Utenti protetti. Questo evento indica che il gruppo di sicurezza non memorizza nella cache le credenziali necessarie per l'autenticazione al server.<br /><br />Visualizza il nome del pacchetto, il nome utente, il nome di dominio e il nome server.|
|304<br /><br />**ProtectedUser-Client**|Motivo: Il pacchetto di sicurezza non archivia le credenziali dell'utente protetta.<br /><br />Un evento informativo viene registrato nel client per indicare che il pacchetto di sicurezza non memorizza nella cache le credenziali di accesso dell'utente. È previsto che il digest (WDigest), la delega delle credenziali (CredSSP) e NTLM non riescano ad avere credenziali di accesso per Utenti protetti. Tuttavia, le applicazioni possono riuscire se fanno richiesta delle credenziali.<br /><br />Visualizza il nome del pacchetto, il nome utente e il nome di dominio.|
|100<br /><br />**ProtectedUserFailures-DomainController**|Motivo: Si verifica un errore di accesso NTLM per un account incluso nel gruppo di sicurezza Utenti protetti.<br /><br />Viene registrato un errore nel controller di dominio per indicare che l'autenticazione NTLM non è riuscita a causa dell'appartenenza dell'account al gruppo di sicurezza Utenti protetti.<br /><br />Visualizza il nome dell'account e il nome del dispositivo.|
|104<br /><br />**ProtectedUserFailures-DomainController**|Motivo: Per l'autenticazione Kerberos viene usato il tipo di crittografia DES o RC4 e si verifica un errore di accesso per un utente nel gruppo di sicurezza Utenti protetti.<br /><br />La preautenticazione Kerberos non riesce perché i tipi di crittografia DES e RC4 non possono essere usati quando l'account è membro del gruppo di sicurezza Utenti protetti.<br /><br />(AES viene accettato).|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|Motivo: Un ticket di concessione ticket (TGT) di Kerberos è stato emesso correttamente per un membro del gruppo Utenti protetti.|



## <a name="additional-resources"></a>Risorse aggiuntive

-   [Gestione e protezione delle credenziali](credentials-protection-and-management.md)

-   [Criteri di autenticazione e silo di criteri di autenticazione](authentication-policies-and-authentication-policy-silos.md)

-   [Come configurare gli account protetti](how-to-configure-protected-accounts.md)


