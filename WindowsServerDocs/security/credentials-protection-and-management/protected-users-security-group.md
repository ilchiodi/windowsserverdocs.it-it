---
title: Gruppo di sicurezza utenti protetti
description: Protezione di Windows Server
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="protected-users-security-group"></a>Gruppo di sicurezza utenti protetti

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento destinato ai professionisti IT descrive il gruppo di sicurezza utenti protetti di Active Directory e viene spiegato come funziona. Questo gruppo è stato introdotto in Windows Server 2012 R2 controller di dominio.

## <a name="BKMK_ProtectedUsers"></a>Panoramica

Questo gruppo di sicurezza è progettato come parte di una strategia per gestire l'esposizione delle credenziali all'interno dell'organizzazione. I membri di questo gruppo dispongono automaticamente protezioni non configurabili applicate al proprio account. L'appartenenza al gruppo utenti protetti è progettata per essere restrittiva e sicura in modo proattivo per impostazione predefinita. L'unico metodo per modificare queste protezioni per un account consiste nel rimuovere l'account dal gruppo di sicurezza.

> [!WARNING]
> Gli account per i servizi e i computer non devono essere mai membri del gruppo utenti protetti. Questo gruppo sarebbe offre protezione incompleta comunque perché la password o il certificato è sempre disponibile nell'host. Autenticazione avrà esito negativo con il nome utente \"the di errore o la password incorrect\" per qualsiasi servizio o un computer in cui viene aggiunto al gruppo utenti protetti.

Questo gruppo globale, relative ai domini attiva una protezione non configurabile in dispositivi e computer host che eseguono Windows Server 2012 R2 e Windows 8.1 o versione successiva per gli utenti nei domini con un controller di dominio primario che esegue Windows Server 2012 R2. In questo modo si riduce il footprint di memoria predefinita delle credenziali quando gli utenti accesso in ingresso ai computer con queste protezioni.

Per ulteriori informazioni, vedere [come gli utenti protetti gruppo funziona](#BKMK_HowItWorks) in questo argomento.



## <a name="BKMK_Requirements"></a>Requisiti di gruppo utenti protetti
I requisiti per la protezione di dispositivo per i membri del gruppo utenti protetti includono:

- Il gruppo di sicurezza globale utenti protetti viene replicato in tutti i controller di dominio nel dominio account.

- Windows 8.1 e Windows Server 2012 R2 aggiunto il supporto per impostazione predefinita. [Advisory Microsoft sulla sicurezza 2871997](https://technet.microsoft.com/library/security/2871997) aggiunge il supporto per Windows 7, Windows Server 2008 R2 e Windows Server 2012.

I requisiti per la protezione con controller di dominio per i membri del gruppo utenti protetti includono:

- Gli utenti devono essere in domini di Windows Server 2012 R2 o versione successiva livello funzionale del dominio.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Gruppo di sicurezza globale utenti protetti aggiunta ai domini di livello inferiore

Controller di dominio che eseguono un sistema operativo precedente a Windows Server 2012 R2 può supportare l'aggiunta di membri al nuovo gruppo di sicurezza utenti protetti. Ciò consente agli utenti di beneficiare delle protezioni dispositivo prima che il dominio viene aggiornato. 

> [!Note]
> I controller di dominio non supportano le protezioni di dominio. 

Gruppo utenti protetti può essere creato mediante [trasferire il ruolo di emulatore dominio primario (PDC) controller](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) a un controller di dominio che esegue Windows Server 2012 R2. Dopo tale oggetto gruppo viene replicato in altri controller di dominio, il ruolo emulatore PDC può essere ospitato in un controller di dominio che esegue una versione precedente di Windows Server.

### <a name="BKMK_ADgroup"></a>Proprietà di gruppo Active Directory gli utenti protette

Nella seguente tabella specifica le proprietà del gruppo utenti protetti.

|Attributo|Valore|
|-------|-----|
|SID/RID noto|S-1-5-21 -<domain>-525|
|Tipo|Dominio globale|
|Contenitore predefinito|CN = Users, DC =<domain>, DC =|
|Membri predefiniti|Nessuno|
|Membro predefinito di|Nessuno|
|Protetto da ADMINSDHOLDER?|No|
|È sicuro spostarlo fuori dal contenitore predefinito?|Sì|
|È sicuro delegare la gestione di questo gruppo di amministratori non di servizio?|No|
|Diritti utente predefiniti|Nessun diritto utente predefinito|

## <a name="BKMK_HowItWorks"></a>Come funziona il gruppo utenti protetti
In questa sezione viene spiegato come funziona il gruppo utenti protetti quando:

-   Registrati in un dispositivo Windows

-   Dominio dell'account utente è in un maggiore livello di funzionalità dominio Windows Server 2012 R2

### <a name="device-protections-for-signed-in-protected-users"></a>Connesso protezioni di dispositivo per utenti protetti
Quando l'utente ha effettuato l'accesso è un membro del gruppo utenti protetti vengono applicate le seguenti protezioni:

-   Credenziali verrà delega (CredSSP) non cache le credenziali dell'utente come testo normale, anche se il **Consenti delega credenziali predefinite** è abilitata l'impostazione di criteri di gruppo.

-   A partire da Windows 8.1 e Windows Server 2012 R2, Digest di Windows verrà non memorizzare nella cache le credenziali in testo normale dell'utente anche quando è abilitato il Digest di Windows.

> [!Note]
> Dopo l'installazione [Advisory Microsoft sulla sicurezza 2871997](https://technet.microsoft.com/library/security/2871997) Digest di Windows continuerà alle credenziali nella cache fino a quando non è configurata la chiave del Registro di sistema. Vedere [Advisory Microsoft sulla sicurezza: aggiornamento per migliorare la gestione e protezione delle credenziali: 13 maggio 2014](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) per le istruzioni.

-   NTLM non memorizza nella cache le credenziali dell'utente come testo normale o una funzione unidirezionale NT (NTOWF).

-   Kerberos non creerà le chiavi di crittografia DES o RC4. Anche il verrà non memorizzare nella cache le credenziali in testo normale o chiavi a lungo termine dell'utente dopo il TGT iniziale viene acquisito.

-   Un sistema di verifica memorizzato nella cache non viene creato al momento dell'accesso o sbloccare, in modo non in linea sign-in non è più supportata.

Dopo aver aggiunto l'account utente al gruppo utenti protetti, protezione inizieranno quando l'utente accede al dispositivo.

### <a name="domain-controller-protections-for-protected-users"></a>Protezioni di controller di dominio per utenti protetti
Gli account che sono membri del gruppo utenti protetti che eseguono l'autenticazione a un dominio Windows Server 2012 R2 sono in grado di:

-   L'autenticazione con l'autenticazione NTLM.

-   Utilizzare i tipi di crittografia DES o RC4 nella preautenticazione Kerberos.

-   Essere delegati con la delega vincolata o non vincolata.

-   Rinnovare i ticket di concessione ticket Kerberos oltre la durata iniziale di quattro ore.

Le impostazioni non configurabili per la scadenza del ticket di concessione ticket vengono stabilite per ciascun account nel gruppo utenti protetti. In genere, il controller di dominio imposta la durata del ticket di concessione ticket e il rinnovo, in base ai criteri di dominio, **durata massima ticket utente** e **durata massima rinnovo ticket utente**. Per il gruppo utenti protetti, viene impostato 600 minuti per i criteri di dominio.

Per ulteriori informazioni, vedere [come configurare gli account protetti](how-to-configure-protected-accounts.md).

## <a name="troubleshooting"></a>Risoluzione dei problemi
Due registri amministrativi sono disponibili per la risoluzione di eventi correlati a utenti protetti. Questi nuovi registri si trovano nel Visualizzatore eventi sono disabilitati per impostazione predefinita e si trovano in **applicazioni e servizi\microsoft\windows\microsoft\autenticazione**.

|ID e registro eventi|Descrizione|
|----------|--------|
|104<br /><br />**ProtectedUser Client**|Motivo: Pacchetto di protezione nel client non contiene le credenziali.<br /><br />L'errore viene registrato nel computer client quando l'account è un membro del gruppo di sicurezza utenti protetti. Questo evento indica che il pacchetto di sicurezza non memorizza nella cache le credenziali necessarie per l'autenticazione nel server.<br /><br />Visualizza il nome del pacchetto, nome utente, nome di dominio e nome del server.|
|304<br /><br />**ProtectedUser Client**|Motivo: Pacchetto di sicurezza non archivia le credenziali dell'utente protetto.<br /><br />Un evento informativo viene registrato nel client per indicare che il pacchetto di sicurezza non memorizza nella cache delle credenziali di accesso dell'utente. È previsto che Digest (WDigest), la delega delle credenziali (CredSSP) e NTLM non riescano ad avere credenziali di accesso per utenti protetti. Le applicazioni possono riuscire se fanno richiesta delle credenziali.<br /><br />Visualizza il nome del pacchetto, un nome utente e un nome di dominio.|
|100<br /><br />**ProtectedUserFailures-DomainController**|Motivo: Un errore di accesso NTLM si verifica per un account nel gruppo di sicurezza utenti protetti.<br /><br />Un errore viene registrato nel controller di dominio per indicare che l'autenticazione NTLM non è riuscita perché l'account è un membro del gruppo di sicurezza utenti protetti.<br /><br />Visualizza il nome dell'account e il nome del dispositivo.|
|104<br /><br />**ProtectedUserFailures-DomainController**|Motivo: I tipi di crittografia DES o RC4 vengono usati per l'autenticazione Kerberos e si verifica un errore di accesso per un utente nel gruppo di sicurezza utenti protetti.<br /><br />La preautenticazione Kerberos non riuscita perché i tipi di crittografia DES e RC4 non possono essere utilizzati quando l'account è un membro del gruppo di sicurezza utenti protetti.<br /><br />(AES viene accettato).|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|Motivo: Un Kerberos ticket-concessione-ticket (TGT) è stato emesso correttamente per un membro del gruppo utenti protetti.|



## <a name="additional-resources"></a>Risorse aggiuntive

-   [Gestione e protezione delle credenziali](credentials-protection-and-management.md)

-   [Criteri di autenticazione e silo di criteri di autenticazione](authentication-policies-and-authentication-policy-silos.md)

-   [Come configurare gli account protetti](how-to-configure-protected-accounts.md)


