---
title: Risoluzione dei problemi di autenticazione
description: Questo argomento fa parte della Guida distribuire accesso remoto con l'autenticazione OTP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e3aa3bf89a73f944b2922fcf06ac60df2874a221
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853654"
---
# <a name="troubleshooting-authentication-issues"></a>Risoluzione dei problemi di autenticazione

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento contiene informazioni sulla risoluzione dei problemi relativi ai problemi riscontrati dagli utenti durante il tentativo di connessione a DirectAccess mediante l'autenticazione OTP. Gli eventi correlati a OTP DirectAccerss vengono registrati nel computer client in Visualizzatore eventi in **registri applicazioni e servizi/Microsoft/Windows/OtpCredentialProvider**. Assicurarsi che questo log sia abilitato per la risoluzione dei problemi relativi a OTP di DirectAccess.  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>Non è stato possibile accedere alla CA che rilascia certificati OTP  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client). Registrazione del certificato OTP per l'utente <username> non riuscita nel server CA < CA_name >, richiesta non riuscita. possibili cause dell'errore: Impossibile risolvere il nome del server della CA. non è possibile accedere al server CA tramite il primo Tunnel DirectAccess oppure non è possibile stabilire la connessione al server della CA.  
  
**Causa**  
  
L'utente ha fornito una password monouso valida e il server DirectAccess ha firmato la richiesta di certificato. Tuttavia, il computer client non è in grado di contattare la CA che rilascia i certificati OTP per completare il processo di registrazione.  
  
**Soluzione**  
  
Nel server DirectAccess eseguire i comandi seguenti di Windows PowerShell:  
  
1.  Ottenere l'elenco delle autorità di certificazione di emissione OTP configurate e controllare il valore di "caserver": `Get-DAOtpAuthentication`  
  
2.  Verificare che le autorità di certificazione siano configurate come server di gestione: `Get-DAMgmtServer -Type All`  
  
3.  Verificare che il computer client abbia stabilito il tunnel dell'infrastruttura: nel Windows Firewall con la console di sicurezza avanzata, espandere **associazioni monitoraggio/sicurezza**, fare clic su **modalità principale**e assicurarsi che le associazioni di sicurezza IPsec vengano visualizzate con gli indirizzi remoti corretti per la configurazione di DirectAccess.  
  
## <a name="directaccess-server-connectivity-issues"></a>Problemi di connettività del server DirectAccess  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client)  
  
Uno degli errori seguenti:  
  
-   Impossibile stabilire una connessione al server di accesso remoto < DirectAccess_server_hostname > utilizzando il percorso di base < OTP_authentication_path > e la porta < OTP_authentication_port. Codice di errore: < > internal_error_code.  
  
-   Impossibile inviare le credenziali utente al server di accesso remoto < DirectAccess_server_hostname > utilizzando il percorso di base < OTP_authentication_path > e la porta < OTP_authentication_port. Codice di errore: < > internal_error_code.  
  
-   Non è stata ricevuta alcuna risposta dal server di accesso remoto < DirectAccess_server_hostname > usando il percorso di base < OTP_authentication_path > e la porta < OTP_authentication_port. Codice di errore: < > internal_error_code.  
  
**Causa**  
  
Il computer client non è in grado di accedere al server DirectAccess tramite Internet, a causa di problemi di rete o di un server IIS configurato in modo non configurato nel server DirectAccess.  
  
**Soluzione**  
  
Verificare che la connessione Internet nel computer client sia funzionante e assicurarsi che il servizio DirectAccess sia in esecuzione e accessibile tramite Internet.  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>Non è stato possibile eseguire la registrazione per il certificato di accesso OTP DirectAccess  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client). La registrazione del certificato dalla CA < CA_name > non è riuscita. La richiesta non è stata firmata come previsto dal certificato di firma OTP oppure l'utente non è autorizzato a eseguire la registrazione.  
  
**Causa**  
  
La password monouso fornita dall'utente è corretta, ma l'autorità di certificazione emittente (CA) ha rifiutato di emettere il certificato di accesso OTP. La richiesta di certificato potrebbe non essere firmata correttamente con l'EKU corretto (criterio dell'applicazione OTP Registration Authority) oppure l'utente non dispone dell'autorizzazione "registrazione" per il modello di OTP DA.  
  
**Soluzione**  
  
Assicurarsi che gli utenti OTP di DirectAccess dispongano delle autorizzazioni per la registrazione per il certificato di accesso OTP di DirectAccess e che il "criterio applicazione" appropriato sia incluso nel modello di firma dell'autorità di registrazione OTP. Verificare inoltre che il certificato dell'autorità di registrazione DirectAccess nel server di accesso remoto sia valido. Vedere 3,2 pianificare il modello di certificato OTP e 3,3 pianificare il certificato dell'autorità di registrazione.  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>Certificato account computer mancante o non valido  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client).  Non è possibile completare l'autenticazione OTP perché non è possibile trovare il certificato del computer necessario per OTP nell'archivio certificati del computer locale.  
  
**Causa**  
  
Per l'autenticazione OTP DirectAccess è necessario un certificato del computer client per stabilire una connessione SSL con il server DirectAccess. Tuttavia, il certificato del computer client non è stato trovato o non è valido, ad esempio, se il certificato è scaduto.  
  
**Soluzione**  
  
Verificare che il certificato del computer esista e che sia valido:  
  
1.  Nel computer client, nella console di MMC Certificates, per l'account computer locale, aprire **Personal/Certificates**.  
  
2.  Verificare che sia presente un certificato emesso che corrisponda al nome del computer e fare doppio clic sul certificato.  
  
3.  Nella finestra di dialogo **certificato** , nella scheda **percorso certificato** , in **stato certificato**, verificare che "questo certificato sia OK".  
  
Se non viene trovato un certificato valido, eliminare il certificato non valido (se esistente) ed eseguire di nuovo la registrazione per il certificato del computer eseguendo `gpupdate /Force` da un prompt dei comandi con privilegi elevati o riavviando il computer client.  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>CA mancante che rilascia i certificati OTP  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client). Non è possibile completare l'autenticazione OTP perché il server DA non ha restituito un indirizzo di una CA emittente.  
  
**Causa**  
  
Non sono presenti CA che emettono certificati OTP configurati o tutte le autorità di certificazione configurate che emettono certificati OTP non rispondono.  
  
**Soluzione**  
  
1.  Usare il comando seguente per ottenere l'elenco delle autorità di certificazione che emettono certificati OTP (il nome della CA viene visualizzato in caserver): `Get-DAOtpAuthentication`.  
  
2.  Se non sono configurate CA:  
  
    1.  Usare il comando `Set-DAOtpAuthentication` o la console di gestione accesso remoto per configurare le autorità di certificazione che emettono il certificato di accesso OTP di DirectAccess.  
  
    2.  Applicare la nuova configurazione e forzare i client ad aggiornare le impostazioni dell'oggetto Criteri di gruppo DirectAccess eseguendo `gpupdate /Force` da un prompt dei comandi con privilegi elevati o riavviando il computer client.  
  
3.  Se sono presenti CA configurate, assicurarsi che siano online e che rispondano alle richieste di registrazione.  
  
## <a name="misconfigured-directaccess-server-address"></a>Indirizzo server DirectAccess non configurato correttamente  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client). Non è possibile completare l'autenticazione OTP come previsto. Impossibile determinare il nome o l'indirizzo del server di accesso remoto.  Codice di errore: < > error_code. Le impostazioni di DirectAccess devono essere convalidate dall'amministratore del server.  
  
**Causa**  
  
L'indirizzo del server DirectAccess non è configurato correttamente.  
  
**Soluzione**  
  
Controllare l'indirizzo del server DirectAccess configurato utilizzando `Get-DirectAccess` e correggere l'indirizzo se non è configurato correttamente.  
  
Verificare che le impostazioni più recenti vengano distribuite nel computer client eseguendo `gpupdate /force` da un prompt dei comandi con privilegi elevati o riavviare il computer client.  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>Non è stato possibile generare la richiesta di certificato di accesso OTP  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client). Impossibile inizializzare la richiesta di certificato per l'autenticazione OTP. Non è possibile generare una chiave privata oppure l'utente <username> non può accedere al modello di certificato < OTP_template_name > sul controller di dominio.  
  
**Causa**  
  
Questo errore può essere dovuto a due cause:  
  
-   L'utente non è autorizzato a leggere il modello di accesso OTP.  
  
-   Il computer dell'utente non può accedere al controller di dominio a causa di problemi di rete.  
  
**Soluzione**  
  
-   Esaminare l'impostazione delle autorizzazioni per il modello di accesso OTP e verificare che tutti gli utenti di cui è stato eseguito il provisioning per OTP DirectAccess dispongano dell'autorizzazione di lettura.  
  
-   Verificare che il controller di dominio sia configurato come server di gestione e che il computer client possa raggiungere il controller di dominio tramite il tunnel dell'infrastruttura. Vedere 3,2 pianificare il modello di certificato OTP.  
  
## <a name="no-connection-to-the-domain-controller"></a>Nessuna connessione al controller di dominio  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client). Non è possibile stabilire una connessione con il controller di dominio per l'autenticazione OTP. Codice di errore: < > error_code.  
  
**Causa**  
  
Questo errore può essere dovuto a due cause:  
  
-   Il computer dell'utente non dispone di connettività di rete.  
  
-   Il controller di dominio non è accessibile tramite il tunnel dell'infrastruttura.  
  
**Soluzione**  
  
-   Verificare che il controller di dominio sia configurato come server di gestione eseguendo il comando seguente da un prompt di PowerShell: `Get-DAMgmtServer -Type All`.  
  
-   Verificare che il computer client possa raggiungere il controller di dominio tramite il tunnel dell'infrastruttura.  
  
## <a name="otp-provider-requires-challengeresponse"></a>Il provider OTP richiede la richiesta di verifica/risposta  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client). L'autenticazione OTP con il server di accesso remoto (< DirectAccess_server_name >) per l'utente (<username>) ha richiesto una richiesta di verifica da parte dell'utente.  
  
**Causa**  
  
Il provider OTP usato richiede che l'utente fornisca credenziali aggiuntive sotto forma di uno scambio di richiesta/risposta RADIUS, che non è supportato da OTP DirectAccess di Windows Server 2012.  
  
**Soluzione**  
  
Configurare il provider OTP in modo da non richiedere la richiesta di verifica/risposta in qualsiasi scenario.  
  
## <a name="incorrect-otp-logon-template-used"></a>Utilizzato modello di accesso OTP errato  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client). Il modello della CA da cui l'utente <username> richiesto un certificato non è configurato per emettere certificati OTP.  
  
**Causa**  
  
Il modello di accesso OTP DirectAccess è stato sostituito e il computer client sta provando a eseguire l'autenticazione usando un modello precedente.  
  
**Soluzione**  
  
Verificare che il computer client stia usando la configurazione OTP più recente eseguendo una delle operazioni seguenti:  
  
-   Forzare un aggiornamento Criteri di gruppo eseguendo il comando seguente da un prompt dei comandi con privilegi elevati: `gpupdate /Force`.  
  
-   Riavviare il computer client.  
  
## <a name="missing-otp-signing-certificate"></a>Certificato di firma OTP mancante  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client). Impossibile trovare un certificato di firma OTP. La richiesta di registrazione del certificato OTP non può essere firmata.  
  
**Causa**  
  
Impossibile trovare il certificato di firma OTP di DirectAccess nel server di accesso remoto. Pertanto, la richiesta di certificato utente non può essere firmata dal server di accesso remoto. Non è presente alcun certificato di firma o il certificato di firma è scaduto e non è stato rinnovato.  
  
**Soluzione**  
  
Eseguire questi passaggi nel server di accesso remoto.  
  
1.  Controllare il nome del modello di certificato di firma OTP configurato eseguendo il cmdlet di PowerShell `Get-DAOtpAuthentication` ed esaminare il valore di `SigningCertificateTemplateName`.  
  
2.  Utilizzare lo snap-in MMC certificati per assicurarsi che nel computer esista un certificato valido registrato da questo modello.  
  
3.  Se non esiste alcun certificato di questo tipo, eliminare il certificato scaduto (se esistente) e registrarsi per un nuovo certificato basato su questo modello.  
  
Per creare il modello di certificato di firma OTP, vedere 3,3 pianificare il certificato dell'autorità di registrazione.  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>UPN/DN mancante o non corretto per l'utente  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (registro eventi client)  
  
Uno degli errori seguenti:  
  
-   Non è possibile autenticare l'utente <username> con OTP. Verificare che sia definito un UPN per il nome utente in Active Directory. Codice di errore: < > error_code.  
  
-   Non è possibile autenticare l'utente <username> con OTP. Verificare che sia definito un DN per il nome utente in Active Directory. Codice di errore: < > error_code.  
  
**Errore ricevuto** (registro eventi del server)  
  
Il nome utente <username> specificato per l'autenticazione OTP non esiste.  
  
**Causa**  
  
L'utente non dispone degli attributi nome entità utente (UPN) o nome distinto (DN) impostati correttamente nell'account utente, queste proprietà sono necessarie per il corretto funzionamento di OTP di DirectAccess.  
  
**Soluzione**  
  
Utilizzare la console utenti e computer Active Directory sul controller di dominio per verificare che entrambi gli attributi siano impostati correttamente per l'utente che esegue l'autenticazione.  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>Il certificato OTP non è considerato attendibile per l'accesso  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Causa**  
  
La CA che rilascia i certificati OTP non si trova nell'archivio NTAuth aziendale; Pertanto, i certificati registrati non possono essere utilizzati per l'accesso. Questa situazione può verificarsi in ambienti a più domini e a più foreste in cui l'attendibilità delle CA tra domini non è stabilita.  
  
**Soluzione**  
  
Verificare che il certificato della radice della gerarchia CA che rilascia i certificati OTP sia installato nell'archivio certificati NTAuth Enterprise del dominio al quale l'utente sta tentando di eseguire l'autenticazione.  
  
## <a name="windows-could-not-verify-user-credentials"></a>Impossibile verificare le credenziali utente  
**Scenario**. L'utente non è in grado di eseguire l'autenticazione con OTP. errore: "autenticazione non riuscita a causa di un errore interno"  
  
**Errore ricevuto** (computer client). Si è verificato un errore durante la verifica delle credenziali da Windows. Riprovare o chiedere assistenza all'amministratore.  
  
**Causa**  
  
Il protocollo di autenticazione Kerberos non funziona quando il certificato di accesso OTP DirectAccess non include un CRL. Il certificato di accesso OTP di DirectAccess non include un CRL perché:  
  
-   Il modello di accesso OTP DirectAccess è stato configurato con l'opzione **non includere le informazioni di revoca nei certificati emessi**.  
  
-   La CA è configurata in modo da non pubblicare CRL.  
  
**Soluzione**  
  
1.  Per confermare la presenza di questo errore, nella console di gestione accesso remoto, nel **passaggio 2 server di accesso remoto**, fare clic su **modifica**, quindi nell'installazione guidata del **server di accesso remoto** fare clic su **modelli di certificato OTP**. Prendere nota del modello di certificato usato per la registrazione dei certificati rilasciati per l'autenticazione OTP. Aprire la console autorità di certificazione, nel riquadro sinistro fare clic su **modelli di certificato**, quindi fare doppio clic sul certificato di accesso OTP per visualizzare le proprietà del modello di certificato.  
  
    Per risolvere questo problema, configurare un certificato per il certificato di accesso OTP e non selezionare la casella di controllo **non includere le informazioni di revoca nei certificati rilasciati** nella scheda **Server** della finestra di dialogo Proprietà modello.  
  
2.  Nel server CA aprire la MMC Autorità di certificazione, fare clic con il pulsante destro del mouse sulla CA emittente e scegliere **Proprietà**. Nella scheda **estensioni** verificare che la pubblicazione CRL sia configurata correttamente.  
  


