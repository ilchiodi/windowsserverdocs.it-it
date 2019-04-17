---
title: Gestione e protezione delle credenziali
description: Protezione di Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e457229c-0126-40fe-948c-101c943e1b57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 16cc1f2260bf0552da6902dc3e97de65d29c7931
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="credentials-protection-and-management"></a>Gestione e protezione delle credenziali

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento, destinato ai professionisti IT descrive le funzionalità e i metodi introdotti in Windows Server 2012 R2 e Windows 8.1 per la protezione delle credenziali e controlli di autenticazione di dominio per ridurre il furto di credenziali.

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>Modalità di amministrazione limitata per connessione Desktop remoto
Modalità di amministrazione limitata offre un metodo di accesso in modo interattivo a un server host remoto senza trasmissione delle credenziali al server. Ciò impedisce che le credenziali di intercettazione durante il processo di connessione iniziale se il server sia stato compromesso.

Usa questa modalità con credenziali di amministratore, il client desktop remoto tenta di accesso in modo interattivo a un host che supporta anche questa modalità senza inviare le credenziali. Quando l'host verifica che l'account utente si connette con diritti di amministratore e supporta la modalità di amministrazione limitata, la connessione ha esito positivo. In caso contrario, il tentativo di connessione ha esito negativo. Modalità di amministrazione limitata non punto invio normale testo o altri formati riutilizzabili delle credenziali a computer remoti.

### <a name="lsa-protection"></a>Protezione LSA
La sicurezza autorità locale (LSA), che si trova all'interno del processo LSASS Local Security Authority Security Service (), convalida gli utenti per gli accessi locali e remoti e impone i criteri di sicurezza locali. Il sistema operativo Windows 8.1 offre protezione aggiuntiva per LSA per impedire l'inserimento di codice da processi non protetti. Ciò offre protezione aggiuntiva per le credenziali archiviate e gestite in LSA. Questa impostazione dei processo protetto per LSA può essere configurata in Windows 8.1, ma è attivata per impostazione predefinita in Windows RT 8.1 e non può essere modificata.

Per informazioni sulla configurazione della protezione LSA, vedere [Configuring Additional LSA Protection](configuring-additional-lsa-protection.md).

### <a name="protected-users-security-group"></a>Gruppo di sicurezza utenti protetti
Questo nuovo gruppo globale di dominio attiva una nuova protezione non configurabile sui dispositivi e computer host che eseguono Windows Server 2012 R2 e Windows 8.1. Il gruppo utenti protetti Abilita protezioni aggiuntive per i controller di dominio e domini nei domini di Windows Server 2012 R2. In questo modo si riduce i tipi di credenziali disponibili quando gli utenti sono connessi al computer della rete da un computer non compromesso.

I membri del gruppo utenti protetti sono limitati aggiuntive tramite i metodi di autenticazione seguenti:

-   Un membro del gruppo utenti protetti può accedere solo tramite il protocollo Kerberos. L'account non può autenticarsi con NTLM, l'autenticazione del Digest o CredSSP. In un dispositivo che esegue Windows 8.1, le password non vengono memorizzate, in modo che il dispositivo che usa uno dei seguenti Security Support Provider (SSP) non riuscirà ad autenticarsi a un dominio, quando l'account è un membro del gruppo utenti protetti.

-   Il protocollo Kerberos non usa i tipi di crittografia più vulnerabili DES o RC4 nel processo di preautenticazione. Ciò significa che il dominio deve essere configurato per supportare almeno il pacchetto di crittografia AES.

-   L'account dell'utente non può essere delegato con il protocollo Kerberos vincolata o non vincolata delega. Ciò significa che le connessioni precedenti ad altri sistemi possono non riuscire se l'utente è membro del gruppo utenti protetti.

-   Impostazione della durata Kerberos Ticket di concessione ticket (TGT) di quattro ore è configurabile tramite criteri di autenticazione e silo, accessibili tramite il Active Directory amministrativi centro. Ciò significa che quando è trascorso quattro ore, l'utente deve ripetere l'autenticazione.

> [!WARNING]
> Account per servizi e computer non devono essere membri del gruppo utenti protetti. Questo gruppo non fornisce protezione locale perché la password o il certificato è sempre disponibile nell'host. Authentication will fail with the error "the user name or password is incorrect" for any service or computer that is added to the Protected Users group.

Per ulteriori informazioni su questo gruppo, vedere [gruppo di sicurezza utenti protetti](protected-users-security-group.md).

### <a name="authentication-policy-and-authentication-policy-silos"></a>Criteri di autenticazione e silo di criteri di autenticazione
Sono introdotti i criteri basati su foresta di Active Directory e possono essere applicate agli account in un dominio con un livello di funzionalità del dominio Windows Server 2012 R2. Questi criteri di autenticazione possono controllare quali host di un utente può utilizzare per l'accesso. Funzionano in combinazione con il gruppo di sicurezza utenti protetti e gli amministratori possono applicare condizioni di controllo di accesso per l'autenticazione agli account. Questi criteri di autenticazione isolano account correlati per limitare l'ambito di una rete.

La nuova classe di oggetti di Active Directory, criteri di autenticazione, consente di applicare la configurazione dell'autenticazione alle classi di account in domini con un livello di funzionalità del dominio Windows Server 2012 R2. Criteri di autenticazione sono applicati durante Kerberos AS o TGS exchange. Classi di account Active Directory sono:

-   Utente

-   Computer

-   Account del servizio gestito

-   Account del servizio gestito di gruppo

Per ulteriori informazioni, vedere [criteri di autenticazione e silo di criteri di autenticazione](authentication-policies-and-authentication-policy-silos.md).

Per ulteriori informazioni su come configurare gli account protetti, vedere [come configurare gli account protetti](how-to-configure-protected-accounts.md).

## <a name="see-also"></a>Vedere anche
For more information about the LSA and the LSASS, see the [Windows Logon and Authentication Technical Overview](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx).



