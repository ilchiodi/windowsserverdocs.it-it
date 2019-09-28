---
title: PASSAGGIO 4 installare e configurare RSA e EDGE1
description: 'Questo argomento fa parte della Guida al Lab di test: dimostrazione di DirectAccess con autenticazione OTP e RSA SecurID per Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d46ede6f-1a21-414d-b8c3-6b5c87344b9d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3519e9d6e89b26a733b6b0178334f86e284bddfb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404757"
---
# <a name="step-4-install-and-configure-rsa-and-edge1"></a>PASSAGGIO 4 installare e configurare RSA e EDGE1

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

RSA è il server RADIUS e OTP e viene installato prima della configurazione di RADIUS e OTP.  
  
Per configurare la distribuzione RSA, attenersi alla procedura seguente:  
  
1. Installare il sistema operativo nel server RSA. Installare Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 nel server RSA.  
  
2. Configurare TCP/IP in RSA. Configurare le impostazioni TCP/IP nel server RSA.  
  
3. Copiare i file di installazione del gestore di autenticazione nel server RSA. Dopo aver installato il sistema operativo in RSA, copiare i file del gestore di autenticazione nel computer RSA.  
  
4. Aggiungere il server RSA al dominio CORP. Aggiungere RSA al dominio CORP.  
  
5. Disabilitare Windows Firewall su RSA. Disabilitare il Windows Firewall nel server RSA.  
  
6. Installare RSA Authentication Manager nel server RSA. Installare RSA Authentication Manager.  
  
7. Configurare RSA Authentication Manager. Configurare il gestore di autenticazione.  
  
8. Creare DAProbeUser. Creare un account utente per scopi di sondaggio.  
  
9. Installare il token del software RSA SecurID in CLIENT1. Installare il token del software RSA SecurID in CLIENT1.  
  
10. Configurare EDGE1 come agente di autenticazione RSA. Configurare l'agente di autenticazione RSA in EDGE1.  
  
11. Configurare EDGE1 per supportare l'autenticazione OTP. Configurare OTP per DirectAccess e verificare la configurazione.  
  
## <a name="InstallOS"></a>Installare il sistema operativo nel server RSA  
  
1.  In RSA avviare l'installazione di Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
2.  Seguire le istruzioni per completare l'installazione, specificando Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 (installazione completa) e una password complessa per l'account amministratore locale. Accedere utilizzando l'account Administrator locale.  
  
3.  Connettere RSA a una rete dotata di accesso a Internet ed eseguire Windows Update per installare gli aggiornamenti più recenti per Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012, quindi disconnettersi da Internet.  
  
4.  Connettere RSA alla subnet Corpnet.  
  
## <a name="TCP"></a>Configurare TCP/IP in RSA  
  
1.  In Attività di configurazione iniziale fare clic su **Configura rete**.  
  
2.  In **connessioni di rete**fare clic con il pulsante destro del mouse su connessione alla rete **locale**e quindi scegliere **proprietà**.  
  
3.  Fare clic su **Protocollo Internet versione 4 (TCP/IPv4)** e quindi su **Proprietà**.  
  
4.  Fare clic su **Utilizza il seguente indirizzo IP**. Nella casella **Indirizzo IP**digitare **10.0.0.5**. In **Subnet mask**digitare **255.255.255.0**. In **gateway predefinito**Digitare **10.0.0.2**. Fare clic su **Usa i seguenti indirizzi server DNS**, in **server DNS preferito**, digitare **10.0.0.1**.  
  
5.  Fare clic su **Avanzate**e quindi sulla scheda **DNS** .  
  
6.  In **suffisso DNS per la connessione**Digitare **Corp.contoso.com**e quindi fare clic su **OK** due volte.  
  
7.  Nella finestra di dialogo **Proprietà connessione alla rete locale** fare clic su **Chiudi**.  
  
8.  Chiudere la finestra **Connessioni di rete**.  
  
## <a name="copyinstfiles"></a>Copiare i file di installazione del gestore di autenticazione nel server RSA  
  
1.  Nel server RSA creare la cartella C:\RSA Installation.  
  
2.  Copiare il contenuto del supporto di RSA Authentication Manager 7,1 SP4 nella cartella di installazione di C:\RSA.  
  
3.  Creare la sottocartella C:\RSA Installation\License e token.  
  
4.  Copiare i file di licenza RSA in C:\RSA Installation\License e token.  
  
## <a name="JoinDomain"></a>Aggiungere il server RSA al dominio CORP  
  
1.  Fare clic con il pulsante destro del mouse su **computer locale**e scegliere **Proprietà**.  
  
2.  Nella scheda **Nome computer** della finestra di dialogo **Proprietà del sistema** fare clic su **Cambia**.  
  
3.  In **nome computer**digitare **RSA**. In **membro di**fare clic su **dominio**, digitare **Corp.contoso.com**, quindi fare clic su **OK**.  
  
4.  Quando viene richiesto di immettere un nome utente e una password, digitare **User1** e la relativa password, quindi fare clic su **OK**.  
  
5.  Nella finestra di dialogo di benvenuto del dominio fare clic su **OK**.  
  
6.  Quando viene richiesto il riavvio del computer, fare clic su **OK**.  
  
7.  Nella finestra di dialogo **Proprietà del sistema** fare clic su **Chiudi**.  
  
8.  Quando viene richiesto di riavviare il computer, fare clic su **Riavvia ora**.  
  
9. Dopo il riavvio del computer, digitare **User1** e la password, selezionare Corp nell'elenco a discesa **Accedi a:** e fare clic su **OK**.  
  
## <a name="BKMK_Firewall"></a>Disabilitare Windows Firewall in RSA  
  
1.  Fare clic sul pulsante **Start**, scegliere **Pannello di controllo**, fare clic su **sistema e sicurezza**e quindi su **Windows Firewall**.  
  
2.  Fare clic **su attiva o disattiva Windows Firewall**.  
  
3.  **Disattivare Windows Firewall** per tutte le impostazioni.  
  
4.  Fare clic su **OK** e chiudere Windows Firewall.  
  
## <a name="install"></a>Installare RSA Authentication Manager nel server RSA  
  
1.  Se il messaggio di avviso di sicurezza viene visualizzato in qualsiasi momento durante il processo, fare clic su **Esegui** per continuare.  
  
2.  Aprire la cartella di installazione di C:\RSA e fare doppio clic su **Autorun. exe**.  
  
3.  Fare clic su **Installa ora**, fare clic su **Avanti**, selezionare l'opzione superiore per le Americhe, quindi fare clic su **Avanti**.  
  
4.  Selezionare Accetto **i termini del contratto di licenza**e fare clic su **Avanti**.  
  
5.  Selezionare **istanza primaria**e fare clic su **Avanti**.  
  
6.  Nel campo **nome directory:** digitare **C:\RSA**, quindi fare clic su **Avanti**.  
  
7.  Verificare che il nome del server (RSA.corp.contoso.com) e l'indirizzo IP siano corretti e fare clic su **Avanti**.  
  
8.  Passare a C:\RSA Installation\License e token, quindi fare clic su **Avanti**.  
  
9. Nella pagina **Verifica file di licenza** , fare clic su **Avanti**.  
  
10. Nel campo **ID utente** digitare **amministratore**e nei campi **password** e **Conferma password** digitare una password complessa. Fare clic su **Avanti**.  
  
11. Nella schermata di selezione dei log accettare le impostazioni predefinite e fare clic su **Avanti**.  
  
12. Nella schermata Riepilogo fare clic su **Installa**.  
  
13. Al termine dell'installazione, fare clic su **fine**.  
  
## <a name="confiauthmgr"></a>Configurare RSA Authentication Manager  
  
1.  Se la console di sicurezza RSA non viene aperta automaticamente, nel desktop del computer RSA fare doppio clic su "RSA Security Console".  
  
2.  Se viene visualizzato l'avviso del certificato di sicurezza/sicurezza, fare clic su **continuare con il sito Web** oppure fare clic su **Sì** per continuare e aggiungere il sito a siti attendibili, se richiesto.  
  
3.  Nel campo **ID utente** digitare **amministratore** e fare clic su **OK**.  
  
4.  Nel campo **password** Digitare la password per l'account amministratore e fare clic **su Accedi**.  
  
5.  Inserire le informazioni sul token.  
  
    1.  Nella **console di sicurezza di RSA** fare clic su **autenticazione** e quindi su **token SecurID**.  
  
    2.  Fare clic su **Importa token processo**, quindi fare clic su **Aggiungi nuovo**.  
  
    3.  Nella sezione **Opzioni di importazione** fare clic su **Sfoglia**. Individuare e selezionare il file XML dei token in C:\ RSA Installation\License e token Folder, quindi fare clic su **Open**.  
  
    4.  Fare clic su **Invia processo** nella parte inferiore della pagina.  
  
6.  Creare un nuovo utente OTP.  
  
    1.  Nella **console di sicurezza di RSA** fare clic sulla scheda **identità** , fare clic su **utenti**e quindi su **Aggiungi nuovo**.  
  
    2.  Nella sezione **Last Name:** digitare **User**e nell' **ID utente:** sezione **User1** (userid deve corrispondere al nome utente di ad usato per questo Lab).  Nelle sezioni **password:** e **Conferma password:** digitare una password complessa. Deselezionare la casella **di controllo "Richiedi all'utente di modificare la password all'accesso successivo"** e fare clic su **Salva**.  
  
7.  Assegnare User1 a uno dei token importati.  
  
    1.  Nella pagina **utenti** fare clic su **User1** e quindi su **token SecurID**.  
  
    2.  Fare clic su **token SecurID** e fare clic su **assegna token**.  
  
    3.  Sotto l'intestazione **numero di serie** fare clic sul primo numero elencato, quindi fare clic su **assegna**.  
  
    4.  Fare clic sul token assegnato e quindi su **modifica**. Nella sezione relativa alla **gestione dei pin di SecurID** per il requisito di **autenticazione utente**selezionare **non richiedere pin (solo codice token)** .  
  
    5.  Fare clic su **Salva e Distribuisci token**.  
  
    6.  Nella sezione **nozioni di base** della pagina **Distribuisci token software** , fare clic su **file token del problema (SDTID)** .  
  
    7.  Nella sezione **Opzioni file token** della pagina **Distribuisci token software** deselezionare la casella di controllo **Abilita protezione copia** . Fare clic su **Nessuna password** e **quindi su Avanti**.  
  
    8.  Nella sezione **file di download** della pagina **Distribuisci token software** fare clic su **Scarica ora**. Fare clic su **Salva**. Passare all'installazione di C:\RSA e fare clic su **Salva** e **Chiudi**.  
  
    9. Ridurre a icona la **console di sicurezza RSA** per usarla in seguito.  
  
8.  Configurare Authentication Manager come server RADIUS.  
  
    1.  Sul desktop RSA computer fare doppio clic su **"RSA Security Operations Console"** .  
  
    2.  Se viene visualizzato l'avviso di sicurezza o avviso del certificato di sicurezza, fare clic su **continuare con il sito Web** oppure fare clic su **Sì** per continuare e aggiungere il sito a siti attendibili, se necessario.  
  
    3.  Immettere l'ID utente e la password e fare clic **su Accedi**.  
  
    4.  Fare clic su **Configurazione distribuzione-RADIUS-Configura server**.  
  
    5.  Nella pagina **credenziali aggiuntive obbligatorie** immettere l'ID utente e la password dell'amministratore e fare clic su **OK**.  
  
    6.  Nella pagina **Configura server RADIUS** immettere la stessa password usata per l'utente amministratore per i **segreti** e la **password master**. Immettere l'ID utente e la password dell'amministratore e fare clic su **Configura**.  
  
    7.  Verificare che sia visualizzato il messaggio **"server RADIUS configurato correttamente"** . Fare clic su **Fine**. Chiudere la **console operatore RSA**.  
  
    8.  Tornare alla **"console di sicurezza RSA"** .  
  
    9. Nella scheda **RADIUS** fare clic su **server RADIUS**. Verificare che rsa.corp.contoso.com sia elencato.  
  
9. Configurare RSA server come client di autenticazione RSA.  
  
    1.  Nella scheda **RADIUS** fare clic su **client RADIUS** e quindi su **Aggiungi nuovo**.  
  
    2.  Fare clic sulla casella di controllo **qualsiasi client RADIUS** .  
  
    3.  Digitare una password complessa a scelta nel campo **segreto condiviso** . Questa password verrà usata in un secondo momento durante la configurazione di EDGE1 per OTP.  
  
    4.  Lasciare vuoto il campo **indirizzo IP** e la voce **marca/modello** come **raggio standard**.  
  
    5.  Fare clic su **Salva senza agente RSA**.  
  
10. Creare i file necessari per la configurazione di EDGE1 come agente di autenticazione RSA.  
  
    1.  Nella scheda **accesso** evidenziare agenti di **autenticazione**e fare clic su **Aggiungi nuovo**.  
  
    2.  Digitare **Edge1** nel campo **nome host** e fare clic su **Risolvi IP**.  
  
    3.  Si noti che l'indirizzo IP per EDGE1 è ora visualizzato nel campo **indirizzo IP** . Fare clic su **Salva**.  
  
11. Generare un file di configurazione per il server EDGE1 (AM_Config. zip).  
  
    1.  Nella scheda **accesso** evidenziare agenti di **autenticazione**e fare clic su **genera file di configurazione**.  
  
    2.  Nella pagina **genera file di configurazione** fare clic su **genera file**di configurazione, quindi fare clic su **Scarica ora**.  
  
    3.  Fare clic su **Salva**, passare a C:\ Installazione RSA, quindi fare clic su **Salva**.  
  
    4.  Fare clic su **Chiudi** nella finestra di dialogo **download completato** .  
  
12. Generare un file Secret del nodo per il server EDGE1 (EDGE1_NodeSecret. zip).  
  
    1.  Nella scheda **accesso** evidenziare agenti di **autenticazione**e fare clic su **Gestisci esistente**.  
  
    2.  Fare clic sul nodo corrente configurato EDGE1, quindi fare clic su **Gestisci il segreto del nodo**.  
  
    3.  Selezionare la casella **di controllo Crea un nuovo segreto del nodo casuale ed esporta il segreto del nodo in un file** .  
  
    4.  Immettere la stessa password usata per l'utente amministratore nei campi **password di crittografia** e **Conferma password di crittografia** e fare clic su **Salva**.  
  
    5.  Nella pagina **generazione file Secret del nodo** , fare clic su **Scarica ora**.  
  
    6.  Nella finestra di dialogo **download del file** fare clic su **Salva**, passare all'installazione di C:\RSA e fare clic su **Salva**. Fare clic su **Chiudi** nella finestra di dialogo **download completato** .  
  
    7.  Dal supporto di RSA Authentication Manager copiare \auth_mgr\windows-x86_64\am\rsa-ace_nsload\win32-5.0-x86\agent_nsload.exe nell'installazione di C:\RSA.  
  
## <a name="BKMK_DAProbeUser"></a>Crea DAProbeUser  
  
1.  Nella **console di sicurezza di RSA** fare clic sulla scheda **identità** , fare clic su **utenti**e quindi su **Aggiungi nuovo**.  
  
2.  Nel tipo di sezione **Last Name:** **Probe**e nella sezione **ID utente:** digitare **DAProbeUser**. Nelle sezioni **password:** e **Conferma password:** digitare una password complessa. Deselezionare la casella **di controllo "Richiedi all'utente di modificare la password all'accesso successivo"** e fare clic su **Salva**.  
  
## <a name="InstToken"></a>Installare il token del software RSA SecurID in CLIENT1  
Usare questa procedura per installare il token software SecurID in CLIENT1.  
  
#### <a name="install-securid-software-token"></a>Installare il token software SecurID  
  
1.  Nel computer CLIENT1 creare la cartella C:\RSA files. Copiare il file Software_Tokens. zip dall'installazione di C:\RSA nel computer RSA ai file C:\RSA. Estrarre il file User1_000031701832. SDTID in C:\RSA files in CLIENT1.  
  
2.  Accedere a RSA SecurID software token source media e fare doppio clic su RSASECURIDTOKEN410 nella cartella **SecurID SoftwareToken client App** per avviare l'installazione di RSA SecurID. Se viene visualizzato il messaggio di **avviso Apri file-sicurezza** , fare clic su **Esegui**.  
  
3.  Nella finestra di dialogo **RSA SecurID software token-InstallShield Wizard** fare clic due volte su **Avanti** .  
  
4.  Accettare il contratto di licenza e fare clic su **Avanti**.  
  
5.  Nella finestra di dialogo **tipo di installazione** selezionare **tipico**, fare clic su **Avanti**e quindi su **Installa**.  
  
6.  Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
7.  Selezionare la casella di controllo **Launch RSA SecurID software token** , quindi fare clic su **Finish**.  
  
8.  Fare clic su **Importa da file**.  
  
9. Fare clic su **Sfoglia**, selezionare C:\RSA Files\User1_000031701832.SDTID e fare clic su **Apri**.  
  
10. Fare clic su **OK** due volte.  
  
## <a name="configAuthAgt"></a>Configurare EDGE1 come agente di autenticazione RSA  
Usare questa procedura per configurare EDGE1 per l'esecuzione dell'autenticazione RSA.  
  
#### <a name="configure-the-rsa-authentication-agent"></a>Configurare l'agente di autenticazione RSA  
  
1. In EDGE1 aprire Esplora risorse e creare la cartella C:\RSA files. Passare al supporto di installazione di RSA ACE.  
  
2. Copiare i file agent_nsload. exe, AM_Config. zip e EDGE1_NodeSecret. zip dal supporto RSA ai file C:\RSA.  
  
3. Estrarre il contenuto di entrambi i file zip nei percorsi seguenti:  
  
   1.  C:\Windows\System32  
  
   2.  C:\Windows\SysWOW64  
  
4. Copiare agent_nsload. exe in C:\Windows\SysWOW64 @ no__t-0.  
  
5. Aprire un prompt dei comandi con privilegi elevati e passare a C:\Windows\SysWOW64.  
  
6. Digitare **agent_nsload. exe-f nodesecret. REC-p <password>** , dove <password> è la password complessa creata durante la configurazione iniziale di RSA. Premi INVIO.  
  
7. Copiare C:\Windows\SysWOW64\securid in C:\Windows\System32.  
  
## <a name="configOTP"></a>Configurare EDGE1 per supportare l'autenticazione OTP  
Usare questa procedura per configurare OTP per DirectAccess e verificare la configurazione.  
  
#### <a name="configure-otp-for-directaccess"></a>Configurare OTP per DirectAccess  
  
1.  In EDGE1 aprire Server Manager e fare clic su **accesso remoto** nel riquadro sinistro.  
  
2.  Fare clic con il pulsante destro del mouse su **Edge1** nel riquadro Server e scegliere **gestione accesso remoto**.  
  
3.  Fare clic su **configurazione**.  
  
4.  Nella finestra **Configurazione DirectAccess** , in **passaggio 2: server di accesso remoto**, fare clic su **modifica**.  
  
5.  Fare clic tre volte su **Avanti** e nella sezione **autenticazione** selezionare **autenticazione a due fattori** e **utilizzare OTP**e assicurarsi che l'opzione **Usa certificati computer** sia selezionata. Verificare che la CA radice sia impostata su **CN = Corp-App1-CA**. Fare clic su **Avanti**.  
  
6.  Nella sezione **server RADIUS OTP** fare doppio clic sul campo **nome server** vuoto.  
  
7.  Nella finestra di dialogo **Aggiungi server RADIUS** digitare **RSA** nel campo **nome server** . Fare clic su **Cambia** accanto al campo **segreto condiviso** e digitare la stessa password usata durante la configurazione dei client RADIUS nel server RSA nel **nuovo segreto** e **confermare i nuovi campi segreti** . Fare clic due volte su **OK** e quindi su **Avanti**.  
  
    > [!NOTE]  
    > Se il server RADIUS si trova in un dominio diverso da quello del server di accesso remoto, il campo **nome server** deve specificare il nome di dominio completo (FQDN) del server RADIUS.  
  
8.  Nella sezione **server CA OTP** selezionare App1.Corp.contoso.com e fare clic su **Aggiungi**. Fare clic su **Avanti**.  
  
9. Nella pagina **modelli di certificato OTP** fare clic su **Sfoglia** per selezionare un modello di certificato usato per la registrazione dei certificati rilasciati per l'autenticazione OTP e nella finestra di dialogo **modelli di certificato** selezionare **DAOTPLogon** . Fare clic su **OK**. Fare clic su **Sfoglia** per selezionare un modello di certificato usato per registrare il certificato usato dal server di accesso remoto per firmare le richieste di registrazione del certificato OTP e nella finestra di dialogo **modelli di certificato** Selezionare **DAOTPRA**. Fare clic su **OK**. Fare clic su **Avanti**.  
  
10. Nella pagina **installazione del server di accesso remoto** fare clic su **fine**e fare clic su **fine** nella **creazione guidata esperto DirectAccess**.  
  
11. Nella finestra di dialogo **Verifica accesso remoto** fare clic su **applica**, attendere l'aggiornamento del criterio DirectAccess e fare clic su **Chiudi**.  
  
12. Nella schermata **Start** Digitare**PowerShell. exe**, fare clic con il pulsante destro del mouse su **PowerShell**, fare clic su **Avanzate**e quindi su **Esegui come amministratore**. Se viene visualizzata la finestra di dialogo **Controllo account utente** , verificare che l'azione visualizzata sia quella desiderata e quindi fare clic su **Sì**.  
  
13. Nella finestra di Windows PowerShell, digitare **gpupdate/force** e premere INVIO.  
  
14. Chiudere e riaprire la console di gestione accesso remoto e verificare che tutte le impostazioni OTP siano corrette.  
  


