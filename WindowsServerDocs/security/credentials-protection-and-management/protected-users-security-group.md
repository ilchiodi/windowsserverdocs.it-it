---
title: Gruppo di sicurezza Utenti protetti
description: Sicurezza di Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0cbec876ebf8a3ce27bf0d6f099ade6a5d6bc032
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403775"
---
# <a name="protected-users-security-group"></a>Gruppo di sicurezza Utenti protetti

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento destinato al professionista IT viene descritto il gruppo di sicurezza Utenti protetti di Active Directory e viene spiegato come funziona. Questo gruppo è stato introdotto nei controller di dominio di Windows Server 2012 R2.

## <a name="BKMK_ProtectedUsers"></a>Panoramica

Questo gruppo di sicurezza è progettato come parte di una strategia per gestire l'esposizione delle credenziali all'interno dell'azienda. Agli account dei membri di questo gruppo vengono applicati automaticamente protezioni non configurabili. Per impostazione predefinita, l'appartenenza al gruppo Utenti protetti è progettata per essere restrittiva e sicura in modo proattivo. L'unico metodo per modificare queste protezioni per gli account consiste nel rimuovere l'account dal gruppo di sicurezza.

> [!WARNING]
> Gli account per i servizi e i computer non devono mai essere membri del gruppo utenti protetti. Questo gruppo fornisce comunque una protezione incompleta poiché la password o il certificato è sempre disponibile nell'host. L'autenticazione avrà esito negativo con l'errore \"il nome utente o la password non è corretta\" per qualsiasi servizio o computer aggiunto al gruppo utenti protetti.

Questo gruppo globale correlato al dominio attiva la protezione non configurabile sui dispositivi e sui computer host che eseguono Windows Server 2012 R2 e Windows 8.1 o versioni successive per gli utenti nei domini con un controller di dominio primario che esegue Windows Server 2012 R2. Questo consente di ridurre notevolmente il footprint di memoria predefinito delle credenziali quando gli utenti eseguono l'accesso ai computer con queste protezioni.

Per ulteriori informazioni, vedere [modalità di funzionamento del gruppo utenti protetti](#BKMK_HowItWorks) in questo argomento.



## <a name="BKMK_Requirements"></a>Requisiti del gruppo utenti protetti
I requisiti per fornire le protezioni dei dispositivi per i membri del gruppo utenti protetti includono:

- Il gruppo di sicurezza globale Utenti protetti viene replicato in tutti i controller di dominio del dominio account.

- In Windows 8.1 e Windows Server 2012 R2 è stato aggiunto il supporto per impostazione predefinita. [Microsoft Security advisor 2871997](https://technet.microsoft.com/library/security/2871997) aggiunge il supporto per Windows 7, windows Server 2008 R2 e windows Server 2012.

I requisiti per la protezione con controller di dominio per i membri del gruppo Utenti protetti includono:

- Gli utenti devono essere in domini che sono Windows Server 2012 R2 o un livello di funzionalità del dominio superiore.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Aggiunta del gruppo di sicurezza globale utente protetto ai domini di livello inferiore

I controller di dominio che eseguono un sistema operativo precedente a Windows Server 2012 R2 possono supportare l'aggiunta di membri al nuovo gruppo di sicurezza utenti protetti. Ciò consente agli utenti di trarre vantaggio dalle protezioni dei dispositivi prima che il dominio venga aggiornato. 

> [!Note]
> I controller di dominio non supporteranno le protezioni di dominio. 

Il gruppo utenti protetti può essere creato [trasferendo il ruolo di emulatore del controller di dominio primario (PDC)](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) in un controller di dominio che esegue Windows Server 2012 R2. Dopo avere replicato l'oggetto gruppo in altri controller di dominio, il ruolo dell'emulatore PDC può essere ospitato in un controller di dominio che esegue una versione precedente di Windows Server.

### <a name="BKMK_ADgroup"></a>Proprietà Active Directory del gruppo utenti protetti

La seguente tabella specifica le proprietà del gruppo Utenti protetti.

|Attributo|Valore|
|-------|-----|
|SID/RID noto|S-1-5-21-<domain>-525|
|Tipo|Dominio globale|
|Contenitore predefinito|CN=Utenti, DC=<domain>, DC=|
|Membri predefiniti|Nessuno|
|Membro predefinito di|Nessuno|
|Protetto da ADMINSDHOLDER?|No|
|È sicuro spostarlo fuori dal contenitore predefinito?|Sì|
|È sicuro delegare la gestione di questo gruppo ad amministratori non di servizio?|No|
|Diritti utente predefiniti|Nessun diritto utente predefinito|

## <a name="BKMK_HowItWorks"></a>Funzionamento del gruppo utenti protetti
In questa sezione viene spiegato come funziona il gruppo Utenti protetti quando:

-   Firmato in un dispositivo Windows

-   Il dominio dell'account utente si trova in un livello di funzionalità del dominio di Windows Server 2012 R2 o superiore

### <a name="device-protections-for-signed-in-protected-users"></a>Protezioni dei dispositivi per gli utenti con accesso protetto
Quando l'utente connesso è un membro del gruppo utenti protetti, vengono applicate le protezioni seguenti:

-   La delega delle credenziali (CredSSP) non memorizza nella cache le credenziali del testo normale dell'utente anche quando è abilitata l'impostazione **Consenti delega criteri di gruppo credenziali predefinite** .

-   A partire da Windows 8.1 e Windows Server 2012 R2, Windows digest non memorizza nella cache le credenziali del testo normale dell'utente anche quando è abilitato il digest di Windows.

> [!Note]
> Dopo l'installazione di [Microsoft Security advisor 2871997](https://technet.microsoft.com/library/security/2871997) , Windows digest continuerà a memorizzare nella cache le credenziali fino alla configurazione della chiave del registro di sistema. Vedere l' [avviso di sicurezza Microsoft: aggiornamento per migliorare la protezione e la gestione delle credenziali: 13 maggio 2014](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) per istruzioni.

-   NTLM non memorizza nella cache le credenziali del testo normale dell'utente o la funzione unidirezionale NT (NTOWF).

-   Kerberos non creerà più le chiavi DES o RC4. Inoltre, le credenziali del testo normale dell'utente o le chiavi a lungo termine non vengono memorizzate nella cache dopo l'acquisizione del TGT iniziale.

-   Un verificatore memorizzato nella cache non viene creato durante l'accesso o lo sblocco, quindi l'accesso offline non è più supportato.

Dopo che l'account utente è stato aggiunto al gruppo utenti protetti, la protezione viene avviata quando l'utente accede al dispositivo.

### <a name="domain-controller-protections-for-protected-users"></a>Protezioni dei controller di dominio per utenti protetti
Gli account che sono membri del gruppo utenti protetti che eseguono l'autenticazione a un dominio Windows Server 2012 R2 non sono in grado di:

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
|104<br /><br />**ProtectedUser-Client**|Motivo: il pacchetto di sicurezza nel client non contiene le credenziali.<br /><br />L'errore viene registrato nel computer client quando l'account è membro del gruppo di sicurezza Utenti protetti. Questo evento indica che il gruppo di sicurezza non memorizza nella cache le credenziali necessarie per l'autenticazione al server.<br /><br />Visualizza il nome del pacchetto, il nome utente, il nome di dominio e il nome server.|
|304<br /><br />**ProtectedUser-Client**|Motivo: il pacchetto di sicurezza non archivia le credenziali dell'utente protetto.<br /><br />Un evento informativo viene registrato nel client per indicare che il pacchetto di sicurezza non memorizza nella cache le credenziali di accesso dell'utente. È previsto che il digest (WDigest), la delega delle credenziali (CredSSP) e NTLM non riescano ad avere credenziali di accesso per Utenti protetti. Tuttavia, le applicazioni possono riuscire se fanno richiesta delle credenziali.<br /><br />Visualizza il nome del pacchetto, il nome utente e il nome di dominio.|
|100<br /><br />**ProtectedUserFailures-DomainController**|Motivo: si verifica un errore di accesso NTLM per un account che si trova nel gruppo di sicurezza utenti protetti.<br /><br />Viene registrato un errore nel controller di dominio per indicare che l'autenticazione NTLM non è riuscita a causa dell'appartenenza dell'account al gruppo di sicurezza Utenti protetti.<br /><br />Visualizza il nome dell'account e il nome del dispositivo.|
|104<br /><br />**ProtectedUserFailures-DomainController**|Motivo: i tipi di crittografia DES o RC4 vengono usati per l'autenticazione Kerberos e si verifica un errore di accesso per un utente nel gruppo di sicurezza utenti protetti.<br /><br />La preautenticazione Kerberos non riesce perché i tipi di crittografia DES e RC4 non possono essere usati quando l'account è membro del gruppo di sicurezza Utenti protetti.<br /><br />(AES viene accettato).|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|Motivo: un ticket di concessione ticket (TGT) Kerberos è stato emesso correttamente per un membro del gruppo utenti protetti.|



## <a name="additional-resources"></a>Risorse aggiuntive

-   [Gestione e protezione delle credenziali](credentials-protection-and-management.md)

-   [Criteri di autenticazione e silo di criteri di autenticazione](authentication-policies-and-authentication-policy-silos.md)

-   [Come configurare gli account protetti](how-to-configure-protected-accounts.md)


