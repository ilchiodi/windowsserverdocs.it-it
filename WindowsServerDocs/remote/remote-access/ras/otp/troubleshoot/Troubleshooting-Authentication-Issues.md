---
title: Risoluzione dei problemi di autenticazione
description: Questo argomento fa parte della Guida alla distribuzione di accesso remoto con autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 08dd6822cc30135506d82041cfbeab0bc1a058ab
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282362"
---
# <a name="troubleshooting-authentication-issues"></a>Risoluzione dei problemi di autenticazione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento contiene informazioni sulla risoluzione dei problemi relativi ai problemi agli utenti potrebbero essere quando si prova a connettersi usando l'autenticazione OTP di DirectAccess. OTP DirectAccerss correlati gli eventi vengono registrati nel computer client nel Visualizzatore eventi **applicazioni e i log di servizi/Microsoft/Windows/OtpCredentialProvider**. Assicurarsi che questo log è abilitato la risoluzione dei problemi con OTP di DirectAccess.  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>Non è stato possibile accedere alla CA che rilascia certificati OTP  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client). Registrazione di certificato OTP per l'utente <username> non riuscita nel server di autorità di certificazione < nome_CA >, richieste non riuscite, possibili cause dell'errore: Nome del server Autorità di certificazione non può essere risolto, il server di autorità di certificazione non è possibile accedervi sul primo tunnel DirectAccess o non è possibile stabilire la connessione al server di autorità di certificazione.  
  
**Causa**  
  
L'utente ha specificato una password monouso valida e il server DirectAccess firmato la richiesta di certificato; Tuttavia, il computer client non riesce a contattare l'autorità di certificazione che rilascia certificati OTP per completare il processo di registrazione.  
  
**Soluzione**  
  
Nel server DirectAccess, eseguire i comandi di Windows PowerShell seguenti:  
  
1.  Ottiene l'elenco di OTP configurate le CA emittenti e controllare il valore di 'CAServer': `Get-DAOtpAuthentication`  
  
2.  Assicurarsi che il server accesso client siano configurati come un server di gestione: `Get-DAMgmtServer -Type All`  
  
3.  Assicurarsi che il computer client ha stabilito il tunnel dell'infrastruttura: Nella finestra di Windows console Firewall con sicurezza avanzata, espandere **delle associazioni di monitoraggio o della sicurezza**, fare clic su **modalità principale**e assicurarsi che le associazioni di sicurezza IPsec vengono visualizzati con remoto corretto indirizzi per la configurazione di DirectAccess.  
  
## <a name="directaccess-server-connectivity-issues"></a>Problemi di connettività DirectAccess server  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client)  
  
Uno dei seguenti errori:  
  
-   Impossibile stabilire una connessione al server di accesso remoto < DirectAccess_server_hostname > usando il percorso di base < OTP_authentication_path > e la porta < OTP_authentication_port >. Codice di errore: < internal_error_code >.  
  
-   Server di accesso remoto < DirectAccess_server_hostname > usando il percorso di base < OTP_authentication_path > e la porta < OTP_authentication_port > non è possibile inviare le credenziali dell'utente. Codice di errore: < internal_error_code >.  
  
-   Non è stata ricevuta una risposta dal server di accesso remoto < DirectAccess_server_hostname > usando il percorso di base < OTP_authentication_path > e la porta < OTP_authentication_port >. Codice di errore: < internal_error_code >.  
  
**Causa**  
  
Il computer client non è possibile accedere al server DirectAccess su Internet, a causa di problemi di rete o a un server IIS non configurato correttamente nel server DirectAccess.  
  
**Soluzione**  
  
Assicurarsi che la connessione Internet nel computer client funzioni e assicurarsi che il servizio di DirectAccess è in esecuzione e accessibile tramite Internet.  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>Non è riuscito a registrare il certificato di accesso OTP di DirectAccess  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client). Registrazione del certificato da autorità di certificazione < nome_CA > non è riuscita. La richiesta non è stata firmata come previsto per il certificato di firma di OTP, oppure l'utente non dispone dell'autorizzazione per registrare.  
  
**Causa**  
  
La password monouso fornita dall'utente non è corretta, ma ha rifiutato l'autorità di certificazione emittente (CA) emettere il certificato di accesso OTP. La richiesta di certificato non sia firmata correttamente con l'EKU corretto (criterio di applicazione dell'autorità di registrazione OTP), l'utente dispone o meno l'autorizzazione "Enroll" nel modello DA OTP.  
  
**Soluzione**  
  
Assicurarsi che gli utenti di OTP di DirectAccess dispongano dell'autorizzazione per registrare il certificato di accesso OTP di DirectAccess e la corretta "dell'applicazione dei criteri" viene incluso nel modello di firma l'autorità di registrazione DA OTP. Assicurarsi anche che il certificato dell'autorità di registrazione di DirectAccess nel server di accesso remoto sia valido. Vedere il modello di certificato OTP di pianificare 3.2 e 3.3 piano il certificato dell'autorità di registrazione.  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>Certificato dell'account computer mancante o non valido  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client).  Impossibile completare l'autenticazione OTP perché il certificato del computer necessario per OTP non viene trovato nell'archivio certificati computer locale.  
  
**Causa**  
  
L'autenticazione OTP di DirectAccess richiede un certificato del computer client di stabilire una connessione SSL con il server DirectAccess. Tuttavia, il certificato del computer client non è stato trovato o non è valido, ad esempio, se il certificato scaduto.  
  
**Soluzione**  
  
Assicurarsi che il certificato di computer esiste ed è valido:  
  
1.  Nel computer client, nella console MMC certificati, per l'account Computer locale, aprire **personale/certificati**.  
  
2.  Assicurarsi che vi sia un certificato rilasciato che corrisponda al nome del computer e fare doppio clic sul certificato.  
  
3.  Nel **certificato** finestra di dialogo il **Certificate Path** nella scheda **lo stato del certificato**, assicurarsi che sia specificato "questo certificato è OK".  
  
Se non viene trovato un certificato valido, eliminare il certificato non valido (se presente) e nuovamente la registrazione per il certificato del computer da entrambi in esecuzione `gpupdate /Force` da un prompt dei comandi con privilegi elevati o riavviare il computer client.  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>Manca l'autorità di certificazione che rilascia certificati OTP  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client). Impossibile completare l'autenticazione OTP perché il server DA non ha restituito un indirizzo di una CA emittente.  
  
**Causa**  
  
Non sono presenti CA che rilasciano certificati OTP configurati o tutte le CA configurata che emettono certificati OTP sono non risponde.  
  
**Soluzione**  
  
1.  Usare il comando seguente per ottenere l'elenco di autorità di certificazione che emettono certificati OTP (viene visualizzato il nome di autorità di certificazione in CAServer): `Get-DAOtpAuthentication`.  
  
2.  Se non sono configurati le autorità di certificazione:  
  
    1.  Usare il comando `Set-DAOtpAuthentication` o la console Gestione accesso remoto per configurare le CA che rilasciano la OTP di DirectAccess certificato per l'accesso.  
  
    2.  Applicare la nuova configurazione e forzare i client per aggiornare le impostazioni oggetto Criteri di gruppo DirectAccess eseguendo `gpupdate /Force` da un prompt dei comandi con privilegi elevati o riavviare il computer client.  
  
3.  Se sono presenti le autorità di certificazione configurata, verificare che siano online e risponda alle richieste di registrazione.  
  
## <a name="misconfigured-directaccess-server-address"></a>Indirizzo del server DirectAccess con configurazione non valida  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client). Come previsto, non è possibile completare l'autenticazione OTP. Non è possibile determinare il nome o l'indirizzo del server di accesso remoto.  Codice di errore: < error_code >. Le impostazioni di DirectAccess devono essere convalidate dall'amministratore del server.  
  
**Causa**  
  
L'indirizzo del server DirectAccess non è configurato correttamente.  
  
**Soluzione**  
  
Controllo configurato DirectAccess server indirizzo utilizzando `Get-DirectAccess` e correggere l'indirizzo, se si è configurato correttamente.  
  
Assicurarsi che le impostazioni più recenti vengono distribuite nel computer client eseguendo `gpupdate /force` da un prompt dei comandi con privilegi elevati o riavviare il computer client.  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>Impossibile generare la richiesta di certificato OTP logon  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client). Impossibile inizializzare la richiesta di certificato per l'autenticazione OTP. Non è possibile generare una chiave privata, utente o <username> non può accedere il modello di certificato < OTP_template_name > nel controller di dominio.  
  
**Causa**  
  
Esistono due possibili cause di questo errore:  
  
-   L'utente non dispone dell'autorizzazione per leggere il modello di accesso OTP.  
  
-   Il computer dell'utente non può accedere il controller di dominio a causa di problemi di rete.  
  
**Soluzione**  
  
-   Verificare le autorizzazioni impostazione sul modello di accesso di OTP e assicurarsi che tutti gli utenti di effettuare il provisioning per OTP di DirectAccess sono 'Read' l'autorizzazione.  
  
-   Assicurarsi che il controller di dominio sia configurato come server di gestione e che il computer client possa raggiungere il controller di dominio attraverso il tunnel dell'infrastruttura. 3\.2 piano il modello di certificato OTP, vedere.  
  
## <a name="no-connection-to-the-domain-controller"></a>Nessuna connessione al controller di dominio  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client). Impossibile stabilire una connessione con il controller di dominio ai fini dell'autenticazione OTP. Codice di errore: < error_code >.  
  
**Causa**  
  
Esistono due possibili cause di questo errore:  
  
-   Il computer dell'utente non dispone di alcuna connettività di rete.  
  
-   Il controller di dominio non è accessibile attraverso il tunnel dell'infrastruttura.  
  
**Soluzione**  
  
-   Assicurarsi che il controller di dominio sia configurato come server di gestione eseguendo il comando seguente da un prompt di PowerShell: `Get-DAMgmtServer -Type All`.  
  
-   Assicurarsi che il computer client possa raggiungere il controller di dominio attraverso il tunnel dell'infrastruttura.  
  
## <a name="otp-provider-requires-challengeresponse"></a>Provider di OTP richiede autenticazione challenge/response  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client). L'autenticazione OTP con il server di accesso remoto (< DirectAccess_server_name >) per l'utente (<username>) una richiesta da parte dell'utente.  
  
**Causa**  
  
Il provider OTP usato richiede all'utente di specificare credenziali aggiuntive sotto forma di uno scambio di richiesta/risposta RADIUS, che non è supportato da OTP di DirectAccess di Windows Server 2012.  
  
**Soluzione**  
  
Configurare il provider OTP per non richiedere attesa/risposta in qualsiasi scenario.  
  
## <a name="incorrect-otp-logon-template-used"></a>Modello di accesso OTP corretto usato  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client). Il modello di autorità di certificazione da cui l'utente <username> ha richiesto un certificato non è configurato per emettere certificati OTP.  
  
**Causa**  
  
È stato sostituito il modello di accesso OTP di DirectAccess e il client sta tentando di eseguire l'autenticazione usando un modello precedente.  
  
**Soluzione**  
  
Verificare che il computer client usa la configurazione di OTP più recente eseguendo una delle operazioni seguenti:  
  
-   Forzare un aggiornamento di criteri di gruppo eseguendo il comando seguente da un prompt dei comandi con privilegi elevati: `gpupdate /Force`.  
  
-   Riavviare il computer client.  
  
## <a name="missing-otp-signing-certificate"></a>Manca il certificato di autenticazione OTP  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client). Impossibile trovare un certificato di autenticazione OTP. Impossibile firmare la richiesta di registrazione di certificato OTP.  
  
**Causa**  
  
Impossibile trovare il certificato di firma OTP di DirectAccess nel server di accesso remoto; Pertanto, la richiesta di certificato utente non può essere firmata dal server di accesso remoto. Nessun certificato di firma o il certificato di firma è scaduto e non è stato rinnovato.  
  
**Soluzione**  
  
Eseguire questi passaggi nel server di accesso remoto.  
  
1.  Controllare configurato OTP firma certificate template name eseguendo il cmdlet di PowerShell `Get-DAOtpAuthentication` ed esaminare il valore di `SigningCertificateTemplateName`.  
  
2.  Utilizzare lo snap-in MMC certificati per verificare che un certificato valido registrato da questo modello esista nel computer.  
  
3.  Se il certificato non esiste, eliminare il certificato scaduto (se presente) e si registra un nuovo certificato basato su questo modello.  
  
Per creare la firma di OTP modello di certificato vedere 3.3 piano il certificato dell'autorità di registrazione.  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>Manca o è errata UPN/nome distinto per l'utente  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi del client)  
  
Uno dei seguenti errori:  
  
-   Utente <username> non possono essere autenticati con OTP. Assicurarsi che per il nome utente in Active Directory è definito un UPN. Codice di errore: < error_code >.  
  
-   Utente <username> non possono essere autenticati con OTP. Assicurarsi che per il nome utente in Active Directory è definito un DN. Codice di errore: < error_code >.  
  
**Errore ricevuto** (registro eventi del server)  
  
Il nome utente <username> specificato per l'autenticazione OTP non esiste.  
  
**Causa**  
  
L'utente non ha il nome dell'entità utente (UPN) o gli attributi di nome distinto (DN) impostano correttamente nell'account di utente, queste proprietà sono necessarie per il funzionamento corretto di OTP di DirectAccess.  
  
**Soluzione**  
  
Utilizzare la console Active Directory Users and Computers nel controller di dominio per verificare che entrambi gli attributi siano correttamente impostate per l'utente che esegue l'autenticazione.  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>Certificato OTP non è attendibile per account di accesso  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Causa**  
  
L'autorità di certificazione che rilascia certificati OTP non è presente nell'archivio NTAuth enterprise; di conseguenza, i certificati registrati non possono essere utilizzati per l'accesso. Ciò può verificarsi in più ambienti a più foreste e di dominio in cui non viene stabilita tra domini CA attendibile.  
  
**Soluzione**  
  
Assicurarsi che il certificato della radice della gerarchia di CA che rilascia certificati OTP è installato nell'organizzazione archivio NTAuth certificati del dominio a cui l'utente tenta di eseguire l'autenticazione.  
  
## <a name="windows-could-not-verify-user-credentials"></a>Windows non è stato possibile verificare le credenziali dell'utente  
**Scenario**. Utente non riesce a eseguire l'autenticazione con OTP con l'errore: "Autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (computer Client). Si è verificato anche se Windows è stata la verifica delle credenziali. Riprovare oppure chiedere all'amministratore per assistenza.  
  
**Causa**  
  
Il protocollo di autenticazione Kerberos non funziona quando il certificato di accesso OTP di DirectAccess non include un elenco CRL. Il certificato di accesso OTP di DirectAccess non include un elenco CRL perché entrambi:  
  
-   Il modello di accesso OTP di DirectAccess è stato configurato con l'opzione **non includono le informazioni di revoca dei certificati emessi**.  
  
-   L'autorità di certificazione non è configurato per la pubblicazione CRL.  
  
**Soluzione**  
  
1.  Per confermare la causa di questo errore, nella console di gestione accesso remoto, nella **passaggio 2 Server di accesso remoto**, fare clic su **modificare**e quindi il **configurazione Server di accesso remoto** procedura guidata, fare clic su **modelli di certificato OTP**. Prendere nota del modello di certificato utilizzato per la registrazione dei certificati emessi per l'autenticazione OTP. Aprire la console Autorità di certificazione, nel riquadro sinistro, fare clic su **modelli di certificato**, fare doppio clic sul certificato di accesso OTP per visualizzare le proprietà del modello di certificato.  
  
    Per risolvere questo problema, configurare un certificato per il certificato di accesso OTP e non si seleziona il **non includono le informazioni di revoca dei certificati emessi** casella di controllo la **Server** scheda del modello finestra di dialogo proprietà.  
  
2.  Nel server CA, aprire la MMC Autorità di certificazione, fare clic con il pulsante destro la CA emittente e fare clic su **proprietà**. Nel **estensioni** scheda verificare che la pubblicazione di CRL sia configurata correttamente.  
  


