---
title: Gestione e protezione delle credenziali
description: Sicurezza di Windows Server
ms.prod: windows-server
ms.technology: security-credential-protection
ms.topic: article
ms.assetid: e457229c-0126-40fe-948c-101c943e1b57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c836da8f83510e6547e0e182ac06fd2151dd9c41
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857064"
---
# <a name="credentials-protection-and-management"></a>Gestione e protezione delle credenziali

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento per i professionisti IT illustra le funzionalità e i metodi introdotti in Windows Server 2012 R2 e Windows 8.1 per la protezione delle credenziali e i controlli di autenticazione del dominio per ridurre il furto di credenziali.

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>Modalità di amministrazione limitata per Connessione Desktop remoto
La modalità di amministrazione limitata offre un metodo per la registrazione interattiva in un server host remoto senza trasmissione delle credenziali al server. Ciò impedisce l'intercettazione delle credenziali durante il processo iniziale di connessione in caso di compromissione del server.

Se si usa questa modalità con credenziali di amministratore, il client di Desktop remoto tenta di eseguire l'accesso in modo interattivo a un host che supporta a sua volta questa modalità senza la trasmissione di credenziali. Quando l'host verifica che l'account utente in fase di connessione dispone di diritti di amministratore e supporta la modalità di amministrazione limitata, la connessione ha esito positivo. In caso contrario, il tentativo di connessione non riesce. La modalità di amministrazione limitata non invia mai a computer remoti le credenziali in testo normale o in altri formati riutilizzabili.

### <a name="lsa-protection"></a>Protezione LSA
LSA (Local Security Authority), che si trova nel processo del servizio server dell'autorità di sicurezza locale (LSASS, Local Security Authority Security Service), convalida gli utenti per gli accessi locali e remoti e impone i criteri di sicurezza locali. Il sistema operativo Windows 8.1 offre protezione aggiuntiva per LSA per impedire l'inserimento di codice da processi non protetti. In questo modo le credenziali archiviate e gestite in LSA sono più sicure. Questa impostazione del processo protetto per LSA può essere configurata in Windows 8.1, ma è abilitata per impostazione predefinita in Windows RT 8,1 e non può essere modificata.

Per informazioni sulla configurazione della protezione per LSA, vedere [Configuring Additional LSA Protection](configuring-additional-lsa-protection.md).

### <a name="protected-users-security-group"></a>Gruppo di sicurezza Utenti protetti
Questo nuovo gruppo globale di dominio attiva una nuova protezione non configurabile sui dispositivi e sui computer host che eseguono Windows Server 2012 R2 e Windows 8.1. Il gruppo utenti protetti Abilita protezioni aggiuntive per i controller di dominio e i domini nei domini di Windows Server 2012 R2. In questo modo si riduce notevolmente il numero di tipi di credenziali disponibili quando gli utenti accedono ai computer in rete da un computer non compromesso.

Ai membri del gruppo Utenti protetti sono applicate limitazioni aggiuntive tramite i metodi di autenticazione seguenti:

-   Un membro del gruppo Utenti protetti può accedere usando solo il protocollo Kerberos. L'account non può eseguire l'autenticazione usando NTLM, Autenticazione del digest o CredSSP. In un dispositivo che esegue Windows 8.1 le password non vengono memorizzate nella cache, quindi il dispositivo che usa uno di questi provider di supporto della sicurezza (SSP) non riuscirà ad autenticarsi in un dominio quando l'account è membro del gruppo utenti protetti.

-   Il protocollo Kerberos non usa i tipi di crittografia più vulnerabili DES o RC4 nel processo di preautenticazione. Ciò significa che il dominio deve essere configurato per supportare almeno il pacchetto di crittografia AES.

-   Non è possibile delegare l'account dell'utente con la delega vincolata o non vincolata Kerberos. Ciò significa che le connessioni precedenti ad altri sistemi possono non riuscire se l'utente è membro del gruppo Utenti protetti.

-   L'impostazione della durata predefinita TGT (Kerberos Ticket Granting Tickets), pari a quattro ore, può essere configurata con Criteri di autenticazione e silo, accessibili dal Centro di amministrazione di Active Directory. Ciò significa che, dopo che sono trascorse le quattro ore predefinite, l'utente deve ripetere l'autenticazione.

> [!WARNING]
> Gli account per i servizi e i computer non devono essere membri del gruppo Utenti protetti. Questo gruppo non fornisce protezione locale perché la password o il certificato è sempre disponibile sull'host. L'autenticazione avrà esito negativo con l'errore "nome utente o password non corretta" per qualsiasi servizio o computer aggiunto al gruppo utenti protetti.

Per altre informazioni su questo gruppo, vedere [Gruppo di sicurezza Utenti protetti](protected-users-security-group.md).

### <a name="authentication-policy-and-authentication-policy-silos"></a>Criteri di autenticazione e silo di criteri di autenticazione
Vengono introdotti i criteri di Active Directory basati su foresta, che possono essere applicati ad account in un dominio con un livello di funzionalità del dominio di Windows Server 2012 R2. Questi criteri di autenticazione possono controllare quali host possono essere usati da un utente per l'accesso. Vengono usati insieme al gruppo di sicurezza Utenti protetti e gli amministratori possono applicare condizioni di controllo di accesso per l'autenticazione negli account. Grazie a questi criteri di autenticazione è possibile isolare account correlati per limitare l'ambito di una rete.

La nuova classe di oggetti Active Directory, criteri di autenticazione, consente di applicare la configurazione di autenticazione alle classi di account in domini con un livello di funzionalità del dominio di Windows Server 2012 R2. I criteri di autenticazione sono applicati durante lo scambio Kerberos AS o TGS. Di seguito sono riportate le classi di account Active Directory:

-   Utente

-   Computer

-   Account del servizio gestito

-   Account del servizio gestito del gruppo

Per altre informazioni, vedere [Criteri di autenticazione e silo di criteri di autenticazione](authentication-policies-and-authentication-policy-silos.md).

Per altre informazioni su come configurare gli account protetti, vedere [Come configurare gli account protetti](how-to-configure-protected-accounts.md).

## <a name="see-also"></a>Vedere anche
Per altre informazioni su LSA e LSASS, vedere [Panoramica tecnica dell'accesso e dell'autenticazione di Windows](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx).



