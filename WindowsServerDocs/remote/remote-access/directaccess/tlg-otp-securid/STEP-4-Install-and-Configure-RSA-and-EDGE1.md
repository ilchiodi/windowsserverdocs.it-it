---
title: PASSAGGIO 4 di installare e configurare RSA ed EDGE1
description: 'Questo argomento fa parte della Guida al Lab di Test: dimostrare DirectAccess con autenticazione OTP e SecurID RSA per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d46ede6f-1a21-414d-b8c3-6b5c87344b9d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5280a02568305512f868f559fe35d11dc618f2b4
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283066"
---
# <a name="step-4-install-and-configure-rsa-and-edge1"></a>PASSAGGIO 4 di installare e configurare RSA ed EDGE1

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Viene installato prima di configurare RADIUS e OTP RSA è il server RADIUS e OTP.  
  
Si eseguirà la procedura seguente per configurare la distribuzione di RSA:  
  
1. Installare il sistema operativo nel server di RSA. Installare Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 nel server di RSA.  
  
2. Configurare TCP/IP su RSA. Configurare le impostazioni TCP/IP nel server di RSA.  
  
3. Copiare i file di installazione di gestione dell'autenticazione al server di RSA. Dopo aver installato il sistema operativo in RSA, copiare i file di gestore di autenticazione per il computer RSA.  
  
4. Aggiungere il server RSA al dominio CORP. Aggiungere RSA al dominio CORP.  
  
5. Disabilitare Windows Firewall su RSA. Disattivare il Firewall di Windows nel server di RSA.  
  
6. Installare il gestore di autenticazione RSA nel server di RSA. Installare il gestore di autenticazione RSA.  
  
7. Configurare gestore di autenticazione RSA. Configurare gestore di autenticazione.  
  
8. Creare DAProbeUser. Creare un account utente per scopi di probe.  
  
9. Installare il token di software RSA SecurID in CLIENT1. Installare il token di software RSA SecurID in CLIENT1.  
  
10. Configurare EDGE1 come un agente di autenticazione RSA. Configurare l'agente di autenticazione RSA in EDGE1.  
  
11. Configurare EDGE1 per supportare l'autenticazione OTP. Configurare l'autenticazione OTP di DirectAccess e verificare la configurazione.  
  
## <a name="InstallOS"></a>Installare il sistema operativo nel server di RSA  
  
1.  In RSA, avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Seguire le istruzioni per completare l'installazione, specificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa) e una password complessa per l'account amministratore locale. Accedere utilizzando l'account Administrator locale.  
  
3.  Connettersi RSA a una rete che abbia accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 e quindi disconnettersi da Internet.  
  
4.  Connettere RSA alla subnet Corpnet.  
  
## <a name="TCP"></a>Configurare TCP/IP su RSA  
  
1.  Nell'attività di configurazione iniziale, fare clic su **configurare la rete**.  
  
2.  Nella **connessioni di rete**, fare doppio clic su **Local Area Connection**, quindi fare clic su **proprietà**.  
  
3.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
4.  Fare clic su **Utilizza il seguente indirizzo IP**. Nella casella **Indirizzo IP**digitare **10.0.0.5**. In **Subnet mask**digitare **255.255.255.0**. Nelle **Gateway predefinito**, digitare **10.0.0.2**. Fare clic su **utilizza i seguenti indirizzi server DNS**, nel **server DNS preferito**, digitare **10.0.0.1**.  
  
5.  Fare clic su **avanzate**, quindi fare clic sui **DNS** scheda.  
  
6.  Nella **suffisso DNS per la connessione**, digitare **corp.contoso.com**, quindi fare clic su **OK** due volte.  
  
7.  Nel **Local Area Connection Properties** finestra di dialogo, fare clic su **Chiudi**.  
  
8.  Chiudere la finestra **Connessioni di rete**.  
  
## <a name="copyinstfiles"></a>Copiare il file di installazione di gestore di autenticazione per il server RSA  
  
1.  Nel server di RSA creare la cartella di installazione C:\RSA.  
  
2.  Copiare il contenuto dei supporti SP4 7.1 di RSA autenticazione Manager nella cartella di installazione C:\RSA.  
  
3.  Creare la sottocartella C:\RSA Installation\License e il Token.  
  
4.  Copiare i file di licenza RSA da C:\RSA Installation\License e Token.  
  
## <a name="JoinDomain"></a>Aggiungere il server RSA al dominio CORP  
  
1.  Fare doppio clic su **risorse del Computer**, fare clic su **proprietà**.  
  
2.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
3.  Nelle **nome Computer**, digitare **RSA**. In **membro del**, fare clic su **Domain**, digitare **corp.contoso.com**, fare clic su **OK**.  
  
4.  Quando viene chiesto di immettere un nome utente e password, digitare **User1** e la relativa password e fare clic su **OK**.  
  
5.  Nel dominio nella finestra di dialogo di benvenuto fare clic su **OK**.  
  
6.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
8.  Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
9. Dopo il riavvio del computer, digitare **User1** e la password, selezionare CORP nel **accedere a:** nell'elenco a discesa e fare clic su **OK**.  
  
## <a name="BKMK_Firewall"></a>Disabilitare Windows Firewall su RSA  
  
1.  Fare clic su **avviare**, fare clic su **Pannello di controllo**, fare clic su **sistema e sicurezza**, fare clic su **Windows Firewall**.  
  
2.  Fare clic su **attiva o disattiva Windows Firewall**.  
  
3.  **Disattivare Windows Firewall** per tutte le impostazioni.  
  
4.  Fare clic su **OK** e chiudere Windows Firewall.  
  
## <a name="install"></a>Installare il gestore di autenticazione RSA nel server di RSA  
  
1.  Se viene visualizzato il messaggio di avviso di sicurezza in qualsiasi momento durante questo processo, fare clic su **eseguiti** per continuare.  
  
2.  Aprire la cartella di installazione C:\RSA e fare doppio clic su **autorun.exe**.  
  
3.  Fare clic su **Installa ora**, fare clic su **successiva**, selezionare l'opzione top per Americhe e fare clic su **Next**.  
  
4.  Selezionare **accetto i termini del contratto di licenza**, fare clic su **successivo**.  
  
5.  Selezionare **istanza primaria**, fare clic su **successivo**.  
  
6.  Nel **nome Directory:** tipo di campo **C:\RSA**, fare clic su **Avanti**.  
  
7.  Verificare che il nome del server (RSA.corp.contoso.com) e l'indirizzo IP siano corretti e fare clic su **successivo**.  
  
8.  Individuare C:\RSA Installation\License e il Token e fare clic su **successivo**.  
  
9. Nel **verifica file di licenza** pagina, fare clic su **successivo**.  
  
10. Nel **ID utente** tipo di campo **amministratore**e il **Password** e **Conferma Password** campi digitare una password complessa. Fare clic su **Avanti**.  
  
11. Nella schermata di selezione del log, accettare le impostazioni predefinite e fare clic su **successivo**.  
  
12. Nella schermata di riepilogo, fare clic su **installare**.  
  
13. Al termine dell'installazione, fare clic su **fine**.  
  
## <a name="confiauthmgr"></a>Configurare gestore di autenticazione RSA  
  
1.  Se la Console di sicurezza RSA non viene aperto automaticamente, quindi sul desktop del computer RSA doppio clic su "RSA Security Console".  
  
2.  Se la sicurezza del certificato avviso / avviso di sicurezza viene visualizzata, fare clic su **continuare con questo sito Web** oppure fare clic su **Yes** per continuare e aggiungere questo sito ai siti attendibili, se richiesto.  
  
3.  Nel **ID utente** tipo di campo **amministratore** e fare clic su **OK**.  
  
4.  Nel **Password** campo digitare la password per l'account amministratore e fare clic su **Accedi**.  
  
5.  Inserire le informazioni di Token.  
  
    1.  Nel **RSA Security Console** fare clic su **Authentication** e fare clic su **token SecurID**.  
  
    2.  Fare clic su **processo di importazione di token**, quindi fare clic su **Aggiungi nuovo**.  
  
    3.  Nel **opzioni di importazione** sezione click **Sfoglia**. Individuare e selezionare il file XML token in C:\ Installation\License RSA e Token cartella, scegliere **aperto**.  
  
    4.  Fare clic su **invia processo** nella parte inferiore della pagina.  
  
6.  Creare OTP nuovo utente.  
  
    1.  Nel **RSA Security Console** fare clic sul **identità** scheda, fare clic su **utenti**, fare clic su **Aggiungi nuovo**.  
  
    2.  Nel **Last Name:** sezione tipo **utente**e nel **ID utente:** sezione tipo **User1** (ID utente deve essere lo stesso come il nome utente di Active Directory usato per questa esercitazione).  Nel **Password:** e **Conferma Password:** sezioni digitare una password complessa. Cancella il **'Richiedi all'utente di modificare la password all'accesso successivo'** casella di controllo e fare clic su **salvare**.  
  
7.  Assegnare User1 a uno dei token importati.  
  
    1.  Nel **gli utenti** pagina fare clic su **User1** e fare clic su **token SecurID**.  
  
    2.  Fare clic su **token SecurID** e fare clic su **assegnare Token**.  
  
    3.  Sotto il **numero di serie** titolo il primo numero elencato, scegliere **assegnare**.  
  
    4.  Il token assegnato, scegliere **modifica**. Nel **gestione PIN SecurID** sezione per **requisito di autenticazione utente**, selezionare **PIN (solo codice token) non richiedono**.  
  
    5.  Fare clic su **salvare e distribuire Token**.  
  
    6.  Nel **distribuire il Software Token** nella pagina il **nozioni di base** fare clic su **problema Token File (SDTID)** .  
  
    7.  Nel **distribuire Software Token** nella pagina il **opzioni del File di Token** sezione, deselezionare il **abilitare la protezione dalla copia** casella di controllo. Fare clic su **senza specificare la Password** e **successivo**.  
  
    8.  Nel **distribuire Software Token** nella pagina il **Scarica File** fare clic su **Scarica ora**. Fare clic su **Salva**. Passare all'installazione C:\RSA e fare clic su **salvare** e **Chiudi**.  
  
    9. Ridurre al minimo le **RSA Security Console** per un uso successivo.  
  
8.  Configurare l'autenticazione di gestione come server RADIUS.  
  
    1.  Nella finestra di RSA computer desktop fare doppio clic su **"RSA Security Operations Console"** .  
  
    2.  Se la sicurezza del certificato avviso / avviso di sicurezza viene visualizzata, fare clic su **continuare con questo sito Web** oppure fare clic su **Yes** per continuare e aggiungere questo sito ai siti attendibili se richiesto.  
  
    3.  Immettere l'ID utente e Password e fare clic su **Accedi**.  
  
    4.  Fare clic su **configurazione della distribuzione - RADIUS - configurazione Server**.  
  
    5.  Nel **necessarie credenziali aggiuntive** pagina immettere l'ID utente di amministratore e la Password e fare clic su **OK**.  
  
    6.  Nel **configura Server RADIUS** pagina immettere la stessa password usata per l'utente amministratore per il **segreti** e **Master Password**. Immettere l'ID utente di amministratore e la Password e fare clic su **configura**.  
  
    7.  Verificare che il messaggio **'è stato configurato i server RADIUS'** viene visualizzato. Fare clic su **Fine**. Chiudi il **RSA Operations Console**.  
  
    8.  Tornare alla **"RSA Security Console"** .  
  
    9. Nel **RADIUS** scheda, scegliere **i server RADIUS**. Verificare che rsa.corp.contoso.com sia elencato.  
  
9. Configurare il server RSA come Client di autenticazione RSA.  
  
    1.  Nel **RADIUS** scheda, fare clic su **client RADIUS** e **Aggiungi nuovo**.  
  
    2.  Scegliere il **Client RADIUS ANY** casella di controllo.  
  
    3.  Digitare una password complessa scelta nel **Shared Secret** campo. Si utilizzerà la stessa password in un secondo momento quando si configura EDGE1 per OTP.  
  
    4.  Lasciare il **indirizzo IP** campo vuoto e il **marca / modello** voce **RADIUS Standard**.  
  
    5.  Fare clic su **salvare senza agente RSA**.  
  
10. Creare i file necessari per la configurazione EDGE1 come un agente di autenticazione RSA.  
  
    1.  Nel **Access** , fare clic su **gli agenti di autenticazione**, fare clic su **Aggiungi nuovo**.  
  
    2.  Tipo di **EDGE1** nel **Hostname** campo e fare clic su **risoluzione IP**.  
  
    3.  Si noti che l'indirizzo IP per EDGE1 viene ora visualizzato nei **indirizzo IP** campo. Fare clic su **Salva**.  
  
11. Generare un file di configurazione per il server EDGE1 (AM_Config.zip).  
  
    1.  Nel **Access** , fare clic su **gli agenti di autenticazione**, fare clic su **generare File di configurazione**.  
  
    2.  Nel **generare File di configurazione** pagina fare clic su **generare File di configurazione**, quindi fare clic su **Scarica ora**.  
  
    3.  Fare clic su **salvare**, Sfoglia in c:\. Installazione di RSA e fare clic su **salvare**.  
  
    4.  Fare clic su **Close** nel **Download completato** finestra di dialogo.  
  
12. Generare un file del segreto nodo per il server EDGE1 (EDGE1_NodeSecret.zip).  
  
    1.  Nel **Access** , fare clic su **gli agenti di autenticazione**e fare clic su **gestire esistente**.  
  
    2.  Fare clic sul nodo corrente configurato EDGE1 e fare clic su **Gestisci nodo segreto**.  
  
    3.  Verificare i **crea un nuovo segreto nodo casuale ed esportare la chiave privata di nodo in un file** casella di controllo.  
  
    4.  Immettere la stessa password usata per l'utente amministratore nel **Password di crittografia** e **confermare la Password di crittografia** campi e fare clic su **Salva**.  
  
    5.  Nel **nodo segreto File generato** pagina fare clic su **Scarica ora**.  
  
    6.  Nel **del File da scaricare** finestra di dialogo fare clic su **salvare**, passare all'installazione C:\RSA e fare clic su **Salva**. Fare clic su **Close** nel **Download completato** finestra di dialogo.  
  
    7.  Dal \auth_mgr\windows-x86_64\am\rsa-ace_nsload\win32-5.0-x86\agent_nsload.exe copia supporti gestore di autenticazione RSA a C:\RSA installazione.  
  
## <a name="BKMK_DAProbeUser"></a>Creare DAProbeUser  
  
1.  Nel **RSA Security Console** fare clic sul **identità** scheda, fare clic su **utenti**, fare clic su **Aggiungi nuovo**.  
  
2.  Nel **Last Name:** sezione tipo **Probe**e il **ID utente:** sezione tipo **DAProbeUser**. Nel **Password:** e **Conferma Password:** sezioni digitare una password complessa. Cancella il **'Richiedi all'utente di modificare la password all'accesso successivo'** casella di controllo e fare clic su **salvare**.  
  
## <a name="InstToken"></a>Installare il token di software RSA SecurID in CLIENT1  
Utilizzare questa procedura per installare token software SecurID in CLIENT1.  
  
#### <a name="install-securid-software-token"></a>Installare token software SecurID  
  
1.  Il computer CLIENT1, creare la cartella C:\RSA file. Copiare il file Software_Tokens.zip da C:\RSA installazione nel computer RSA C:\RSA file. Estrarre il file User1_000031701832.SDTID ai file C:\RSA in CLIENT1.  
  
2.  Accedere all'origine di token di supporto di software RSA SecurID e fare doppio clic su RSASECURIDTOKEN410 nel **app client SecurID SoftwareToken** cartella da cui iniziare l'installazione di RSA SecurID. Se il **Apri File - avviso di sicurezza** messaggio viene visualizzato, quindi fare clic su **eseguire**.  
  
3.  Nel **RSA SecurID Software Token - installazione guidata InstallShield** finestra di dialogo fare clic su **successivo** due volte.  
  
4.  Accettare il contratto di licenza e fare clic su **successivo**.  
  
5.  Nel **tipo di installazione** finestra di dialogo selezionare **tipica**, fare clic su **successiva**, fare clic su **installare**.  
  
6.  Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
7.  Selezionare il **avviare RSA SecurID Software Token** casella di controllo e fare clic su **fine**.  
  
8.  Fare clic su **Importa da File**.  
  
9. Fare clic su **esplorare**C:\RSA Files\User1_000031701832.SDTID selezionare e fare clic su **Open**.  
  
10. Fare clic su **OK** due volte.  
  
## <a name="configAuthAgt"></a>Configurare EDGE1 come un agente di autenticazione RSA  
Utilizzare questa procedura per configurare EDGE1 per eseguire l'autenticazione di RSA.  
  
#### <a name="configure-the-rsa-authentication-agent"></a>Configurare l'agente di autenticazione RSA  
  
1. In EDGE1 aprire Esplora Windows e creare la cartella C:\RSA file. Passare al supporto di installazione di voce ACE di RSA.  
  
2. Copia agent_nsload.exe il file, AM_Config.zip ed EDGE1_NodeSecret.zip dal supporto di RSA C:\RSA file.  
  
3. Estrarre il contenuto dei file con estensione zip nei percorsi seguenti:  
  
   1.  C:\Windows\system32\  
  
   2.  C:\Windows\SysWOW64\  
  
4. Copy agent_nsload.exe to C:\Windows\SysWOW64\\.  
  
5. Aprire un prompt dei comandi con privilegi elevati e passare a C:\Windows\SysWOW64.  
  
6. Tipo di **nodesecret.rec di agent_nsload.exe -f -p <password>**  in cui <password> è la password complessa creato durante la configurazione iniziale di RSA. Premi INVIO.  
  
7. Copiare C:\Windows\SysWOW64\securid C:\Windows\System32.  
  
## <a name="configOTP"></a>Configurare EDGE1 per supportare l'autenticazione OTP  
Utilizzare questa procedura per configurare l'autenticazione OTP di DirectAccess e verificare la configurazione.  
  
#### <a name="configure-otp-for-directaccess"></a>Configurare l'autenticazione OTP di DirectAccess  
  
1.  Nel EDGE1, aprire Server Manager e fare clic su **accesso remoto** nel riquadro sinistro.  
  
2.  Fare doppio clic su **EDGE1** nel riquadro Server e scegliere **Gestione accesso remoto**.  
  
3.  Fare clic su **configurazione**.  
  
4.  Nel **configurazione di DirectAccess** finestra, sotto **passaggio 2: Server di accesso remoto**, fare clic su **Edit**.  
  
5.  Fare clic su **successivo** tre volte e il **autenticazione** sezione selezionare **a due fattori di autenticazione** e **Usa OTP**e assicurarsi che **Usa certificati computer** sia selezionata. Verificare che la CA radice è impostata su **CN = corp-APP1-CA**. Fare clic su **Avanti**.  
  
6.  Nel **Server RADIUS OTP** , quindi fare doppio clic su bianco **nome Server** campo.  
  
7.  Nel **aggiungere un Server RADIUS** finestra di dialogo, digitare **RSA** nel **nome Server** campo. Fare clic su **Change** accanto al **segreto condiviso** campo, quindi digitare la stessa password usati durante la configurazione dei client RADIUS nel server di RSA nel **nuovo master secret** e **Conferma segreto nuovo** campi. Fare clic su **OK** due volte, fare clic su **successivo**.  
  
    > [!NOTE]  
    > Se il server RADIUS si trova in un dominio diverso da quello del server di accesso remoto, il **nome Server** campo necessario specificare il FQDN del server RADIUS.  
  
8.  Nel **server CA OTP** sezione APP1.corp.contoso.com selezionare e fare clic su **Add**. Fare clic su **Avanti**.  
  
9. Nel **modelli di certificato OTP** pagina fare clic su **Sfoglia** per selezionare un modello di certificato usato per la registrazione dei certificati emessi per l'autenticazione OTP e scegliere il  **I modelli di certificato** della finestra di dialogo select **DAOTPLogon**. Fare clic su **OK**. Fare clic su **esplorare** per selezionare un modello di certificato usato per registrare il certificato utilizzato dal server di accesso remoto per le richieste di registrazione certificato OTP di accesso e sulle **modelli di certificato** nella finestra di dialogo Selezionare **DAOTPRA**. Fare clic su **OK**. Fare clic su **Avanti**.  
  
10. Nel **configurazione Server di accesso remoto** pagina fare clic su **Finish**e fare clic su **fine** nel **guidato Expert DirectAccess**.  
  
11. Nel **verifica di accesso remoto** fare clic su finestra di dialogo **Apply**, attendere l'aggiornamento di criteri DirectAccess e fare clic su **Chiudi**.  
  
12. Nel **avviare** digitare**powershell.exe**, fare doppio clic su **powershell**, fare clic su **avanzate**, fare clic su **runas amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
13. Nella finestra di Windows PowerShell, digitare **gpupdate /force** e premere INVIO.  
  
14. Chiudere e riaprire la Console di gestione accesso remoto e verificare che tutte le impostazioni OTP siano corrette.  
  


