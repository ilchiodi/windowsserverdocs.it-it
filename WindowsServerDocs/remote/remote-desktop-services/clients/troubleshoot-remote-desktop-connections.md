---
title: Risoluzione dei problemi di connessioni Desktop remoto
description: Procedure di risoluzione dei problemi disposti da sintomo
ms.custom: na
ms.reviewer: rklemen;josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: jobende
manager: ''
ms.author: v-tea;kaushika;rklemen;josh.bender
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: 8965df09fd57decf7f4a1b6b89861e5a67034a0a
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297400"
---
# Risoluzione dei problemi di connessioni Desktop remoto
Per una breve spiegazione dei diversi dei più comuni problemi di Servizi Desktop remoto (RDS), vedi [domande frequenti sul client Desktop remoto](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). Questo articolo descrive vari approcci più avanzati per la risoluzione dei problemi di connessione. Molte di queste procedure si applicano se la risoluzione di una semplice configurazione, ad esempio un computer fisico la connessione a un altro computer fisico o una configurazione più complessa. Alcune procedure risolvono i problemi che si verificano solo negli scenari multiutente più complessi. Per altre informazioni sui componenti desktop remoti e come interagiscono tra loro, vedere [l'architettura di Servizi Desktop remoto](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> Molte delle procedure descritti in questo articolo richiedono accesso a più computer, che alcune delle quali potrebbe essere necessario accedere in remoto. Per ulteriori informazioni su come configurarle e strumenti di amministrazione remota, vedi [Server Administration Tools remota per i sistemi operativi Windows](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

Nell'elenco seguente, identificare il tipo di sintomo in cui si verificano puoi (o gli utenti).

- [Il client desktop remoto non riesce a connettersi a desktop remoto, ma non siano presenti sintomi specifici o messaggi (passaggi di risoluzione dei problemi generali)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [Il client desktop remoto non riesce a connettersi a desktop remoto e riceve un messaggio "Classe non registrata"](#client-cannot-connect-class-not-registered)
- [Il client desktop remoto non riesce a connettersi a desktop remoto e non riceve una "licenze disponibili" o "sicurezza" messaggio](#client-cannot-connect-no-licenses-available)
- [L'utente riceve un messaggio "Accesso negato" o deve fornire le credenziali due volte](#user-cannot-authenticate-or-must-authenticate-twice)
- [Sulla connessione, il riceve un messaggio "Servizio di Desktop remoto è occupato"](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [Il client desktop remoto disconnette e non è possibile riconnettere alla stessa sessione](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [L'utente si connette a un portatile remoto tramite una rete wireless, e quindi il portatile Scollega dalla rete](#remote-laptop-disconnects-from-wireless-network).
- [L'utente esperienze prestazioni scarse o problemi con applicazioni remote](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Per un elenco corrente dei codici di disconnessione RDP, vedi [l'enumerazione ExtendedDisconnectReasonCode](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## Nessun sintomi specifici o messaggi (passaggi di risoluzione dei problemi generali)

Usa questi passaggi quando un client Desktop remoto non riesce a connettersi a un desktop remoto, ma non include i messaggi o altri sintomi che potrebbe essere utile identificano la causa. Per risolvere molti delle cause più comuni di questo tipo di problema, Usa i metodi seguenti:

- [Controllare lo stato del protocollo RDP](#check-the-status-of-the-rdp-protocol)
- [Controllare lo stato dei servizi RDP](#check-the-status-of-the-rdp-services)
- [Verificare che il listener RDP funziona](#check-that-the-rdp-listener-is-functioning)
- [Controlla la porta di ascolto RDP](#check-the-rdp-listener-port)

### Controllare lo stato del protocollo RDP

#### Controllare lo stato del protocollo RDP su un computer locale

Per controllare e modificare lo stato del protocollo RDP su un computer locale, informazioni su [come attivare Desktop remoto](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Se non sono disponibili le opzioni di desktop remote, vedi [Controlla se un oggetto Criteri di gruppo blocca RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### Controllare lo stato del protocollo RDP su un computer remoto

> [!IMPORTANT]  
> Segui i passaggi descritti in questa sezione con attenzione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima modificarlo, [eseguire il backup del Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verificano problemi.

Per controllare e modificare lo stato del protocollo RDP su un computer remoto, utilizzare una connessione di rete del Registro di sistema:

1. Seleziona **Start**, selezionare **Esegui**e quindi immettere **regedt32**.
2. Nell'Editor del Registro di sistema, selezionare **File**e quindi seleziona **La connessione del Registro di sistema di rete**.
3. Nella finestra di dialogo **Seleziona Computer** , Immetti il nome del computer remoto, seleziona **I nomi di controllare**e quindi seleziona **OK**.
4. Passa al **Server HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal**.  
   ![Editor del Registro di sistema, che mostra la voce fDenyTSConnections](..\media\troubleshoot-remote-desktop-connections\RegEntry_fDenyTSConnections.png)
   - Se il valore della chiave **fDenyTSConnections** è **0**, RDP è abilitato
   - Se il valore della chiave **fDenyTSConnections** è **1**, RDP è disabilitato
5. Per abilitare RDP, modifica il valore di **fDenyTSConnections** da **1** a **0**.

#### Controlla se un oggetto Criteri di gruppo (GPO) sta bloccando RDP su un computer locale

Se non puoi attivare RDP nell'interfaccia utente o se il valore di **fDenyTSConnections** viene ripristinato a **1** dopo che è stata modificata, un oggetto Criteri di gruppo possono sostituire le impostazioni a livello di computer.

Per controllare la configurazione dei criteri di gruppo in un computer locale, Apri una finestra del prompt dei comandi come amministratore e immetti il comando seguente:
```
gpresult /H c:\gpresult.html
```
Al termine di questo comando, Apri gpresult.html. Nella **Configurazione computer\\modelli amministrativi\\componenti di Components\\Remote Desktop Services\\Remote sessione Desktop Host\\Connections**, trova il criterio **Consenti agli utenti di connettersi in modalità remota tramite Servizi Desktop remoto** .

- Se l'impostazione di questo criterio è **abilitato**, criteri di gruppo non sta bloccando le connessioni RDP.
- Se l'impostazione di questo criterio è **disabilitato**, controlla **Vince oggetto Criteri di gruppo**. Questo è l'oggetto Criteri di gruppo che sta bloccando le connessioni RDP.
![Un segmento di esempio di gpresult.html, in cui l'oggetto di livello di dominio * * RDP blocco * * disabilitazione RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_GP.png)
   
  ![Un segmento di esempio di gpresult.html, in cui * * locale criteri di gruppo * disabilitazione RDP.](..\media\troubleshoot-remote-desktop-connections\GPResult_RDSH_Connections_LGP.png)

#### Controlla se un oggetto Criteri di gruppo blocca RDP su un computer remoto

Per controllare la configurazione di criteri di gruppo in un computer remoto, il comando è quasi uguali a un computer locale:
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
Il file che genera questo comando (**gpresult-\<computer name\>.html**) usa lo stesso formato informazioni come utilizza la versione del computer locale (**gpresult.html**).

#### Modifica di un oggetto Criteri di blocco

Puoi modificare queste impostazioni nel gruppo di criteri oggetto Editor (GPE) e gruppo di criteri Management Console (GPM). Per ulteriori informazioni su come usare criteri di gruppo, vedi [Gestione avanzata Criteri di gruppo](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/).

Per modificare il criterio di blocco, utilizzare uno dei metodi seguenti:

- In GPE, accedere al livello appropriato di oggetti Criteri di gruppo (ad esempio locale o di dominio) e passa a **Configurazione computer\\modelli amministrativi\\componenti di Components\\Remote Desktop Services\\Remote sessione Desktop Host\\Connections**\\Consenti** agli utenti di connettersi in remoto tramite Servizi Desktop remoto**.  
   1. Impostare il criterio su **abilitato** o **Non configurata**.
   2. Nei computer interessati, Apri una finestra del prompt dei comandi come amministratore ed Esegui il comando **gpupdate /force** .
- In GPM, passa all'unità organizzativa in cui viene applicato il criterio di blocco per i computer interessati ed eliminare il criterio dall'unità Organizzativa.

### Controllare lo stato dei servizi RDP

Nel computer locale (client) e il computer remoto (destinazione), dovrebbero essere in esecuzione i servizi seguenti:

- Servizi Desktop remoto (TermService)
- Redirector porta in modalità utente di Servizi Desktop remoto (UmRdpService)

È possibile utilizzare lo snap-in MMC Servizi per gestire i servizi in locale o in remoto. Anche usare PowerShell in locale o in remoto (se il computer remoto è configurato per accettare i comandi di PowerShell remoti).

![Servizi Desktop remoto nello snap-in MMC Servizi. Non modificare le impostazioni predefinite del servizio.](..\media\troubleshoot-remote-desktop-connections\RDSServiceStatus.png)

In entrambi i computer, se uno o entrambi i servizi non sono in esecuzione, avviarli.

> [!NOTE]  
> Se si avvia il servizio di Servizi Desktop remoto, fare clic su **Sì** per il riavvio automatico del servizio Remote Desktop Services in modalità utente porta Redirector.

### Verificare che il listener RDP funziona

> [!IMPORTANT]  
> Segui i passaggi descritti in questa sezione con attenzione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima modificarlo, [eseguire il backup del Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verificano problemi.

#### Controllare lo stato del listener RDP

Per eseguire questa procedura, Usa un'istanza di PowerShell con autorizzazioni amministrative. Per un computer locale, è anche possibile utilizzare un prompt dei comandi con autorizzazioni amministrative. Tuttavia, questa procedura Usa PowerShell perché gli stessi comandi funzionano entrambi in locale e in modalità remota.

1. Apri una finestra di PowerShell. Per connettersi a un computer remoto, immettere **\<computer name\> Enter-PSSession-ComputerName**.
2. Immetti **qwinsta**. 
    ![Il comando qwinsta Elenca i processi in ascolto sulle porte del computer.](..\media\troubleshoot-remote-desktop-connections\WPS_qwinsta.png)
3. Se l'elenco include **rdp tcp** con lo stato di **rimanere in attesa**, il listener RDP funziona. Continua a [controllare la porta di ascolto RDP](#check-the-rdp-listener-port). In caso contrario, continua con il passaggio 4.
4. Esportare la configurazione del listener RDP da un computer funzionante.
    1. Accedi a un computer che ha la stessa versione del sistema operativo, come il computer interessato ha e accedere del Registro di sistema del computer (ad esempio, tramite Editor del Registro di sistema).
    2. Passa alla voce del Registro di sistema seguente:  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. Esportare la voce in un file con estensione reg. Nell'Editor del Registro di sistema, ad esempio, fai la voce, seleziona **esportare**e quindi immetti un nome di file per le impostazioni esportate.
    4. Copia il file. reg esportato al computer interessato.
5. Per importare la configurazione del listener RDP, Apri una finestra di PowerShell con autorizzazioni amministrative nel computer interessato (o aprire la finestra di PowerShell e connettersi in remoto al computer interessati).
   1. Per eseguire il backup la voce del Registro di sistema esistente, Immetti il comando seguente:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Per rimuovere la voce del Registro di sistema esistente, Immetti i seguenti comandi:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Per importare la nuova voce del Registro di sistema e quindi riavviare il servizio, Immetti i seguenti comandi:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      dove \<filename\> è il nome del file reg esportati.
6. Testare la configurazione tentare nuovamente la connessione desktop remoto. Se non è possibile connettersi, riavvia il computer interessato.
7. Se è ancora non riesce a connettersi, [controllare lo stato del certificato autofirmato RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### Controllare lo stato del certificato autofirmato RDP

1. Se non è possibile connettersi, Apri lo snap-in MMC certificati. Quando viene richiesto di selezionare l'archivio certificati per gestire, seleziona **l'account del Computer**e quindi seleziona il computer interessato.
2. Nella cartella **certificati** in **Desktop remoto**, eliminare il certificato autofirmato RDP. 
    ![Certificati di Desktop remoti nello snap-in MMC certificati.](..\media\troubleshoot-remote-desktop-connections\MMCCert_Delete.png)
3. Nel computer interessato, riavviare il servizio di Servizi Desktop remoto.
4. Aggiorna lo snap-in certificati.
5. Se il certificato autofirmato RDP non è stato creato nuovamente, [Controlla le autorizzazioni della cartella MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### Controlla le autorizzazioni della cartella MachineKeys

1. Nel computer interessato, Apri Esplora e quindi passa alla **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\**.
2. Fai **MachineKeys**, scegli **proprietà**, selezionare **protezione**e quindi selezionare **Avanzate**.
3. Assicurati che siano configurate le autorizzazioni seguenti:
      - BUILTIN\\Administrators.: Il controllo completo
      - Tutti gli utenti: Lettura, scrittura

### Controlla la porta di ascolto RDP

Nel computer locale (client) e il computer remoto (destinazione), il listener RDP deve restare in ascolto sulla porta 3389. Altre applicazioni non devono utilizzare questa porta.

> [!IMPORTANT]  
> Segui i passaggi descritti in questa sezione con attenzione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima modificarlo, [eseguire il backup del Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verificano problemi.

Per controllare o modificare la porta RDP, utilizzare l'Editor del Registro di sistema:

1. Fare clic su Start, selezionare **Esegui**e quindi immettere **regedt32**.
      - Per connetterti a un computer remoto, selezionare **File**e quindi seleziona **La connessione del Registro di sistema di rete**.
      - Nella finestra di dialogo **Seleziona Computer** , Immetti il nome del computer remoto, seleziona **I nomi di controllare**e quindi seleziona **OK**.
2. Apri il Registro di sistema e passa alla **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>**. 
    ![La sottochiave NumeroPorta per il protocollo RDP.](..\media\troubleshoot-remote-desktop-connections\RegEntry_PortNumber.png)
3. Se **NumeroPorta** ha un valore diverso da **3389**, modificarla in **3389**. 
   > [!IMPORTANT]  
    > È possibile utilizzare Servizi Desktop remoto con un'altra porta. Tuttavia, si consiglia di non eseguire questa operazione. Risoluzione dei problemi relativi a tale configurazione non rientra nell'ambito di questo articolo.
4. Dopo aver modificato il numero di porta, riavviare il servizio di Servizi Desktop remoto.

#### Controllo che un'altra applicazione non tenta di utilizzare la stessa porta

Per eseguire questa procedura, Usa un'istanza di PowerShell con autorizzazioni amministrative. Per un computer locale, è anche possibile utilizzare un prompt dei comandi con autorizzazioni amministrative. Tuttavia, questa procedura Usa PowerShell perché gli stessi comandi di lavoro in locale e in modalità remota.

1. Apri una finestra di PowerShell. Per connettersi a un computer remoto, immettere **\<computer name\> Enter-PSSession-ComputerName**.
2. Immetti il comando seguente:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![Il comando netstat produce un elenco delle porte e i servizi in ascolto su di essi.](..\media\troubleshoot-remote-desktop-connections\WPS_netstat.png)
1. Cerca una voce per la porta TCP 3389 (o la porta RDP assegnata) con lo stato di **attesa**. 
    > [!NOTE]  
   > Il PID (identificatore di processo) del processo o servizio con tale porta viene visualizzato sotto la colonna PID.
1. Per determinare quale applicazione utilizza la porta 3389 (o la porta RDP assegnata), Immetti il comando seguente:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![Il comando tasklist segnala i dettagli di un processo specifico.](..\media\troubleshoot-remote-desktop-connections\WPS_tasklist.png)
1. Cerca una voce per il numero di PID che è associato alla porta (dall'output **netstat** ). I servizi o processi associati a tale PID vengono visualizzati a destra.
1. Se un'applicazione o servizio diverso da Servizi Desktop remoto (TermServ.exe) usa la porta, è possibile risolvere i conflitti usando uno dei metodi seguenti:
      - Configurare il applicazione o servizio per l'uso di una porta diversa (scelta consigliata).
      - Disinstalla l'altra applicazione o il servizio.
      - Configurare RDP per l'uso di una porta diversa e quindi riavviare il servizio di Servizi Desktop remoto (scelta non consigliato).

#### Controlla se un firewall blocca la porta RDP

Usa lo strumento **psping** per verificare se è possibile raggiungere i computer interessati tramite la porta 3389.

1. In un computer diverso da computer interessato, scaricare **psping** da <https://live.sysinternals.com/psping.exe>.
2. Apri una finestra del prompt dei comandi come amministratore, passare alla directory in cui è stato installato **psping**e quindi immetti il comando seguente:  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Verificare l'output del comando **psping** per ottenere risultati, ad esempio quanto segue:  
      - **Connessione a \<computer IP\>**: il computer remoto è raggiungibile.
      - **(0% persi)**: tutti i tentativi di connessione ha avuto esito positivo.
      - **Il computer remoto ha rifiutato la connessione di rete**: il computer remoto non è raggiungibile.
      - **(100% persi)**: tutti i tentativi di connessione non è riuscito.
1. Eseguire **psping** in più computer per testare la possibilità di connettersi al computer interessato.
1. Nota Se il computer interessato blocca le connessioni da tutti gli altri computer, alcuni altri computer o solo un altro computer.
1. Consigliato passaggi successivi:
      - Coinvolgere gli amministratori di rete per verificare che la rete consenta il traffico RDP al computer interessato.
      - Esaminare le configurazioni di qualsiasi firewall tra il computer di origine e il computer interessato (tra cui Windows Firewall nel computer interessato) per determinare se un firewall blocca la porta RDP.

## Client non riesce a connettersi, "Classe non registrata"

Quando si tenta di connettersi a un computer remoto usando un client che esegue Windows 10, versione 1709 o versioni successive, il client non sia possibile connettersi mentre il server Host sessione Desktop remoto segnala un messaggio che contiene il codice di errore "Classe non registrata (0x80040154)".

Questo problema si verifica se l'utente che sta tentando di connettersi ha un profilo utente obbligatori. Per risolvere questo problema, installare il [24 luglio 2018-KB4338817 (16299.579 Build del sistema operativo)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) aggiornamento di Windows 10.

## Client non riesce a connettersi, senza licenze disponibili

Questa situazione si applica alle distribuzioni che includono un server host sessione Desktop remoto e un server licenze Desktop remoto.

Identifica il tipo di comportamento sono visualizzati gli utenti:

- La sessione è stata disconnessa perché non sono disponibili licenze o è disponibile alcun server licenze
- Accesso negato a causa di un errore di sicurezza

Accedi all'Host sessione Desktop remoto come amministratore di dominio e Apri il Diagnoser di licenza di desktop remoto. Esaminare i messaggi, ad esempio quanto segue:

  - Il periodo di tolleranza per il telecomando server Host sessione desktop è scaduto, ma il server Host sessione Desktop remoto non è stato configurato con qualsiasi server licenze. a meno che non è configurato un server licenze per il server Host sessione Desktop remoto non verranno accettate connessioni per il server Host sessione Desktop remoto.
  - Licenza server \<computer name\> non è disponibile. Ciò può essere causata da problemi di connettività di rete, viene arrestato il servizio Gestione licenze di Desktop remoto nel server licenze o la gestione delle licenze di desktop remoto non più installato nel computer.

Questi problemi tendono a essere associati i messaggi utente seguenti:

  - La sessione remota è stata disconnessa perché sono non disponibili licenze di accesso client Desktop remoto per il computer.
  - La sessione remota è stata disconnessa perché nessun server di licenza di Desktop remoto disponibili per fornire una licenza.

In questo caso, [configurare il servizio di gestione delle licenze di desktop remoto](#configure-the-rd-licensing-service).

Se il Diagnoser di licenza di desktop remoto Elenca altri problemi, ad esempio "il componente del protocollo RDP X.224 ha rilevato un errore nel flusso di protocollo e ha disconnesso il client" potrebbero essere un problema che riguarda i certificati di licenza. Tali problemi tendono a essere associate a messaggi di utente, ad esempio quanto segue:

A causa di un errore di sicurezza, il client Impossibile connettersi al server Terminal. Dopo avere verificato che sono connessi alla rete, riprovare a connettersi al server.

In questo caso, [le chiavi del Registro di sistema dei certificati di X509 verso il basso](#refresh-the-x509-certificate-registry-keys).

### Configurare il servizio di gestione delle licenze di desktop remoto

La procedura seguente usa Server Manager per apportare le modifiche di configurazione. Per informazioni su come configurare e usare Server Manager, vedi [Server Manager](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager).

1. Apri Server Manager e quindi passa a **Servizi Desktop remoto**.
2. **Panoramica della distribuzione**, seleziona **le attività**e quindi selezioni **Modificare le proprietà di distribuzione**.
3. Seleziona **Le licenze Desktop remoto**e quindi seleziona la modalità di licenza appropriata per la distribuzione (**Per dispositivo** o **Per ogni utente**).
4. Immetti il nome di dominio completo (FQDN) del server licenze Desktop remoto e quindi scegli **Aggiungi**.
5. Se hai più di un server di licenze di desktop remoto, Ripeti il passaggio 4 per ogni server. 
    ![Opzioni di configurazione di server licenze Desktop remoto in Server Manager.](..\media\troubleshoot-remote-desktop-connections\RDLicensing_Configure.png)

### Le chiavi del Registro di sistema dei certificati di X509 verso il basso

> [!IMPORTANT]  
> Segui i passaggi descritti in questa sezione con attenzione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima modificarlo, [eseguire il backup del Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verificano problemi.

Per risolvere questo problema, eseguire il backup e Rimuovi il X509 chiavi del Registro di sistema dei certificati, riavviare il computer e server di gestione licenze riattiva il desktop remoto. A tale scopo, segui questi passaggi.

> [!NOTE]  
> Eseguire la procedura seguente su ogni server host sessione Desktop remoto.

1. Apri l'Editor del Registro di sistema e passa a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. Nel menu del Registro di sistema, selezionare **Esporta File del Registro di sistema**.
3. Digita **Certificato esportato** nella casella **nome di File** e quindi selezionare **Salva**.
4. Fai ognuno dei valori seguenti, seleziona **eliminare**e quindi seleziona **Sì** per verificare l'eliminazione:  
      - **Certificato**
      - **Certificati X509**
      - **ID di certificato X509**
      - **X509 Certificate2**
5. Uscire dall'Editor del Registro di sistema e quindi riavviare il server host sessione Desktop remoto.

## Utente non può eseguire l'autenticazione o deve autenticarsi due volte

Esistono diversi problemi che possono causare problemi che influiscono sull'autenticazione utente. Questa sezione analizza i casi seguenti:

  - [Un utente viene negato l'accesso con un messaggio "tipo di accesso limitato"](#access-denied-restricted-type-of-logon)
  - [Un utente o applicazione viene negato l'accesso con l'evento 16965, "una chiamata remota al database SAM è stata negata"](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Un utente non può accedere usando una smart card](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Se il PC remoto è bloccato, un utente deve immettere una password due volte](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Un utente non può accedere e riceve "autenticazione messaggi di errore" e "Correzione oracle di crittografia CredSSP"](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Dopo aver aggiornato i computer client, alcuni utenti devono accedere due volte](#after-you-update-client-computers-some-users-need-to-sign-in-twice).
  - [Gli utenti sono negati l'accesso in una distribuzione che usa RemoteGuard con più Broker di connessione desktop remoto](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### Accesso negato, tipo con restrizioni di accesso

In questo caso, un utente di Windows 10 tentando di connettersi ai computer Windows 10 o Windows Server 2016 viene negato l'accesso con il messaggio seguente:

> Connessione Desktop remoto:  
> L'amministratore di sistema ha limitato il tipo di accesso (rete o interattivo) che è possibile utilizzare. Per assistenza, contattare l'amministratore di sistema o il supporto tecnico.

Questo problema si verifica quando l'autenticazione a livello di rete (NLA) è necessaria per le connessioni RDP e l'utente non è un membro del gruppo di **Utenti Desktop remoto** . Può verificarsi anche se il gruppo di **Utenti Desktop remoto** non è stato assegnato all'utente di **accesso al computer dalla rete** a destra.

Per risolvere questo problema, segui uno di questi passaggi:

  - [Modificare l'appartenenza al gruppo dell'utente o Assegnazione diritti utente](#modify-the-users-group-membership-or-user-rights-assignment).
  - Disattiva NLA (scelta non consigliata).
  - Usare i client desktop remoti diverso da Windows 10. Ad esempio, i client Windows 7 non sono il problema.

#### Modificare l'appartenenza al gruppo dell'utente o Assegnazione diritti utente

Se il problema riguarda un singolo utente, la soluzione più semplice per questo problema consiste nell'aggiungere l'utente al gruppo **Utenti Desktop remoto** .

Se l'utente è già un membro del gruppo (o se più membri del gruppo hanno lo stesso problema), controlla la configurazione di diritti utente sul computer remoto di Windows 10 o Windows Server 2016.

1. Apri gruppo criteri oggetto Editor (GPE) e connettiti al criterio locale del computer remoto.
2. Passa **all'Assegnazione dei diritti di Computer Configuration\\Windows Settings\\Security Settings\\Impostazioni Policies\\User**, fai **accesso al computer dalla rete**e quindi seleziona **proprietà**.
3. Controlla l'elenco di utenti e gruppi di **Utenti Desktop remoto** (o un gruppo padre).
4. Se l'elenco non include **Utenti Desktop remoto** (o un gruppo padre, ad esempio **tutti gli utenti**) devi aggiungerlo all'elenco (se hai più di uno o due computer nella distribuzione, utilizzare un oggetto Criteri di gruppo).  
    Ad esempio, l'appartenenza predefinita per **l'accesso al computer dalla rete** include **tutti gli utenti**. Se la distribuzione Usa un oggetto Criteri di gruppo per rimuovere **tutti gli utenti**, potresti dover ripristinare l'accesso aggiornando l'oggetto Criteri di gruppo per aggiungere **Utenti Desktop remoto**.

### Accesso negato, una chiamata remota al database SAM è stata negata.

Questo comportamento è più probabile si verifichi se i controller di dominio eseguono Windows Server 2016 o versioni successive e gli utenti tentano di connettersi con un'app di connessione personalizzato. In particolare, le applicazioni che accedono alle informazioni del profilo dell'utente in Active Directory verranno negate l'accesso.

Questo comportamento è dovuta a una modifica a Windows. In Windows Server 2012 R2 e versioni precedenti versioni, quando un utente accede a un remote desktop, i contatti di Manager di connessione remota (RCM) il controller di dominio (DC) per eseguire una query configurazioni che sono specifiche per Desktop remoto sull'oggetto utente nel dominio di Active Directory Servizi (AD DS). Queste informazioni viene visualizzate nella scheda profilo Servizi Desktop remoto di proprietà dell'oggetto dell'utente in Active Directory Users e snap-in di MMC di computer.

A partire da Windows Server 2016, RCM query non è più l'oggetto agli utenti in Active Directory Domain Services. Se è necessario venga automaticamente una query di Active Directory Domain Services perché usi gli attributi di Servizi Desktop remoto, è necessario abilitare manualmente il comportamento RCM precedente.

> [!IMPORTANT]  
> Segui i passaggi descritti in questa sezione con attenzione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima modificarlo, [eseguire il backup del Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verificano problemi.

Per abilitare il comportamento legacy RCM in un server Host sessione Desktop remoto, configurare le voci del Registro di sistema seguenti e quindi riavviare il servizio di **Servizi Desktop remoto** :  
  - **Servizi NT\\Terminal HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Winstation name\>\\**  
      - Nome: **fQueryUserConfigFromDC**
      - Tipo: **Reg\_DWORD**
      - Valore: **1** (decimale)

Per abilitare il comportamento legacy RCM in un server diverso da un server Host sessione Desktop remoto, configurare queste voci del Registro di sistema e la voce del Registro di sistema aggiuntivi seguenti (e quindi riavviare il servizio):
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

Per ulteriori informazioni su questo comportamento, vedi KB 3200967 [modifiche alla gestione connessioni Remote in Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### Un utente non può accedere usando una smart card

Questo articolo illustra tre situazioni comuni in cui un utente non può accedere a un desktop remoto con una smart card:  
  - [L'utente non può accedere a una filiale che usa una sola lettura dominio Controller (controller di dominio)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [L'utente non può accedere a un computer Windows Server 2008 SP2 che è stato aggiornato recentemente](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [L'utente non può rimanere connesso a un computer Windows che è stato aggiornato di recente, e il servizio di Servizi Desktop remoto diventa non risponde (si blocca)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### Non possono accedere con una smart card in una filiale con un controller di dominio

Si verifica questo problema nelle distribuzioni che includono un server host sessione Desktop remoto presso un ramo che usa un controller di dominio. Il server host sessione Desktop remoto è ospitato nel dominio radice. Gli utenti nel sito ramo appartengono a un dominio figlio e usano le smart card per l'autenticazione. Il controller è configurato per le password utente cache (il controller di dominio a cui appartiene il **Gruppo di replica Password controller consentito**). Quando gli utenti tentano di accedere a sessioni nel server host sessione Desktop remoto, vengono visualizzati messaggi, ad esempio "tentativo di accesso non è valido. Questo è uno a causa di un nome utente o l'autenticazione di informazioni non validi".

Si tratta di un problema noto in modo che la radice del controller di dominio e il controller di dominio gestire la crittografia delle credenziali utente. La radice del controller di dominio Usa una chiave di crittografia per crittografare le credenziali e il controller di dominio rappresenta una chiave di decrittografia al client. Tuttavia, le due chiavi non corrispondono.

Per risolvere questo problema, è consigliabile modificare la topologia di controller di dominio o disattivando la memorizzazione della password nel controller o tramite la distribuzione di un controller di dominio scrivibile al sito branch. Un metodo alternativo è spostare il server host sessione Desktop remoto allo stesso dominio figlio come gli utenti. Un'alternativa consiste nel consentire agli utenti di accedere senza le smart card. Qualsiasi di queste soluzioni potrebbero richiedere compromettere le prestazioni o del livello di sicurezza.

#### Non possono accedere con una smart card in un computer Windows Server 2008 SP2

Questo problema si verifica quando gli utenti effettuano l'accesso a un computer Windows Server 2008 SP2 che è stato aggiornato con KB4093227 (2018.4B). Quando gli utenti tentano di accedere con una smart card, non hanno accesso con messaggi, ad esempio "Nessun certificati validi trovati. Verifica che la scheda viene inserita correttamente e si adatta strettamente." Allo stesso tempo, il computer Windows Server registra l'evento di applicazione "Errore durante il recupero di un certificato digitale dalla smart card inserita. Firma non valida."

Per risolvere questo problema, aggiorna il computer di Windows Server con il 2018.06Bre-versione di 4093227 KB, [Descrizione dell'aggiornamento della sicurezza per il rifiuto Windows protocollo RDP (Remote Desktop) di vulnerabilità di servizio in Windows Server 2008: 10 aprile 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### Non può rimanere firmato con una smart card e blocchi di servizio di Servizi Desktop remoto

Questo problema si verifica quando gli utenti effettuano l'accesso a un computer Windows o Windows Server che è stato aggiornato con 4056446 KB. Innanzitutto, l'utente potrebbe essere in grado di accedere al sistema utilizzando una smart card, ma viene ricevuto un messaggio di errore "SCARD\_E\_NO\_SERVICE". Computer remoto potrebbe non risponda.

Per risolvere questo problema, riavviare il computer remoto.

Per risolvere questo problema, aggiorna il computer remoto con la correzione appropriata:

  - Windows Server 2008 SP2: KB 4090928, [gestisce il processo di lsm.exe perdite di Windows e applicazioni per smart card potrebbero visualizzare gli errori di "SCARD\_E\_NO\_SERVICE"](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB, 4103724 [17 maggio 2018-KB4103724 (anteprima di cumulativo mensile)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 e Windows 10, versione 1607: 4103720 KB, [17 maggio 2018-KB4103720 (14393.2273 Build del sistema operativo)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### Se il PC remoto è bloccato, un utente deve immettere una password due volte

Questo problema può verificarsi quando un utente tenta di connettersi a un desktop remoto che esegue Windows 10 versione 1709 in una distribuzione in cui le connessioni RDP non richiedono NLA. In queste condizioni, se è stato bloccato il desktop remoto, l'utente deve immettere le proprie credenziali due volte durante la connessione.

Per risolvere questo problema, aggiorna il computer di Windows 10 versione 1709 con 4343893 KB, [30 agosto 2018-KB4343893 (16299.637 Build del sistema operativo)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### Utente non può accedere e riceve "autenticazione messaggi di errore" e "Correzione oracle di crittografia CredSSP"

Quando gli utenti tentano di accedere con qualsiasi versione di Windows da Windows Vista SP2 e versioni successive o Windows Server 2008 SP2 e versioni successive, essi sono negati l'accesso e ricevere messaggi simile al seguente:

  - Si è verificato un errore di autenticazione. La funzione richiesta non è supportata.
  - È possibile che la correzione di oracle crittografia CredSSP

"Correzione oracle di crittografia CredSSP" si riferisce a un set di aggiornamenti della sicurezza rilasciata a marzo e aprile maggio del 2018. CredSSP è un provider di autenticazione che elabora le richieste di autenticazione per le altre applicazioni. Il marzo 13 2018, "3B" e gli aggiornamenti successivi interessato un exploit in cui un utente malintenzionato può inoltrare le credenziali utente per eseguire il codice nel sistema di destinazione.

Gli aggiornamenti iniziali aggiunto il supporto per un nuovo oggetto Criteri di gruppo, **La crittografia Oracle correzione**, che include le seguenti impostazioni possibili:

  - **Vulnerabili**. Le applicazioni client che usano CredSSP possono eseguire il fallback a versioni meno sicure, ma questo comportamento espone il funzionamento dei desktop remoti agli attacchi. Servizi che usano CredSSP accettano i client che non sono stati aggiornati.
  - **Misura di prevenzione**. Alle applicazioni client che usano CredSSP non possono fallback alle versioni non protette, ma i servizi che utilizzano CredSSP accettano i client che non sono stati aggiornati.
  - **Force aggiornato i client**. Le applicazioni client che usano CredSSP non possono fallback alle versioni meno sicure e servizi che usano CredSSP non verranno accettate client senza patch. 
    Nota: Questa impostazione non deve essere distribuita fino a quando non tutti gli host remoti supportano la versione più recente.

L'aggiornamento di 8 maggio 2018, modificato l'impostazione di **Crittografia Oracle correzione** da **Vulnerable** in **motivo attenuato**. Con questa modifica, client desktop remoto con gli aggiornamenti non riesce a connettersi ai server che essi non hanno (o aggiornare i server che non è stati riavviati). Per altre informazioni sugli effetti degli aggiornamenti e i tipi di comunicazione che essi bloccare, vedi 4093492 KB, [CredSSP gli aggiornamenti per CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Per risolvere questo problema, assicurarsi che tutti i sistemi sono completamente aggiornati e riavviati. Per un elenco completo degli aggiornamenti e altre informazioni sulle vulnerabilità, vedere [CVE-2018-0886 | Vulnerabilità di esecuzione di codice remoto CredSSP](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886).

Per risolvere questo problema fino al termine degli aggiornamenti, controlla 4093492 KB per i tipi di connessioni consentiti. Se non sono presenti alcun alternative fattibile si può considerare uno dei metodi seguenti:

  - Per i computer client interessati, impostare i criteri di **Crittografia Oracle correzione** a **Vulnerable**.
  - Modificare i criteri seguenti nella cartella **Configurazione computer\\modelli amministrativi\\componenti di Components\\Remote Desktop Services\\Remote sessione Desktop Host\\Security** criteri di gruppo:  
      - **Richiedi l'uso del livello di protezione specifiche per le connessioni remote (RDP)**: imposta su **Enabled** e seleziona **RDP**.
      - **Richiedi autenticazione utente per le connessioni remote con autenticazione a livello di rete**: impostare su **disabilitato**.
      > [!IMPORTANT]  
      > Queste modifiche riducono la sicurezza della distribuzione. Dovrebbe essere solo temporanee, se scegli di usarle affatto.

Per altre informazioni sull'utilizzo dei criteri di gruppo, vedi [la modifica di un oggetto Criteri di gruppo impediscono l'aggiornamento](#modifying-a-blocking-gpo).

### Dopo aver aggiornato i computer client, alcuni utenti devono accedere due volte

Gli utenti di alcune Windows 7 o Windows 10 versione 1709 Accedi a una sessione desktop remoto e vedere immediatamente una secondo nella schermata di accesso. Questo problema si verifica se il computer client hanno ricevuto gli aggiornamenti seguenti:

  - Windows 7: KB, 4103718 [8 maggio 2018-KB4103718 (aggiornamento cumulativo mensile)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB, 4103727 [8 maggio 2018-KB4103727 (Build del sistema operativo 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Per risolvere questo problema, assicurarsi che i computer che gli utenti a cui connettersi (, nonché i server host sessione Desktop remoto o RDVI) sono completamente aggiornati tramite giugno 2018. Sono inclusi gli aggiornamenti seguenti:

  - Windows Server 2016: KB, 4284880 [12 giugno 2018, ovvero KB4284880 (Build del sistema operativo 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB, 4284815 [12 giugno 2018, ovvero KB4284815 (aggiornamento cumulativo mensile)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB, 4284855 [12 giugno 2018, ovvero KB4284855 (aggiornamento cumulativo mensile)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB, 4284826 [12 giugno 2018, ovvero KB4284826 (aggiornamento cumulativo mensile)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564, [Descrizione dell'aggiornamento della sicurezza per le vulnerabilità di esecuzione di codice remoto CredSSP in Windows Server 2008, Windows Embedded POSReady 2009 e 2009 Standard di Windows Embedded: 13 marzo 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### Gli utenti sono negati l'accesso in una distribuzione che usa Remote Credential Guard con più Broker di connessione desktop remoto

Questo problema si verifica in distribuzioni a disponibilità elevata che utilizzano due o più Broker di connessione Desktop di remoto, se Windows Defender Remote Credential Guard è in uso. Gli utenti non possono accedere ai desktop remoto.

Questo problema si verifica perché Remote Credential Guard usa Kerberos per l'autenticazione e limita NTLM. Tuttavia, in una configurazione della disponibilità elevata con bilanciamento del carico, il broker di connessione desktop remoto non supporta le operazioni Kerberos.

Se hai bisogno di usare la configurazione di una disponibilità elevata con carico bilanciato Broker di connessione desktop remoto, è possibile risolvere questo problema disattivando Remote Credential Guard. Per ulteriori informazioni su come gestire Windows Defender Remote Credential Guard, vedi [le credenziali di Desktop remoto proteggere con Windows Defender Remote Credential Guard](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## Sulla connessione, il messaggio "Servizio di Desktop remoto è occupato"

Per determinare una risposta appropriata per questo problema, Usa i passaggi seguenti:

1. Il servizio di Servizi Desktop remoto diventa non risponde (ad esempio, il client desktop remoto viene visualizzato come "bloccato" nella schermata iniziale).  
      - Se il servizio non risponde, vedere [problema di memoria server host sessione Desktop remoto](#rdsh-server-memory-issue).
      - Se il client sembra essere interagisce con il servizio normalmente, continuare al passaggio successivo.
2. Se uno o più utenti disconnettere le sessioni desktop remoto, gli utenti connettersi nuovamente?  
   - Se il servizio continuerà a impedire le connessioni indipendentemente dal numero di utenti disconnessione le sessioni, vedi [problema listener di desktop remoto](#rd-listener-issue).
   - Se il servizio inizia ad accettare connessioni nuovamente dopo un numero di utenti hanno scollegamento delle sessioni, [controllare i criteri di limite di connessione](#check-the-connection-limit-policy).

### Problema di memoria server host sessione Desktop remoto

In alcuni server host sessione Desktop remoto di Windows Server 2012 R2 è stata trovata una perdita di memoria. Nel tempo, questi server iniziano a rifiutano le connessioni desktop remoto sia accessi console locale con i messaggi, ad esempio quanto segue:

> Impossibile completare l'attività che si sta tentando di eseguire il servizio di Desktop remoto è occupato. Provare nuovamente in pochi minuti. Altri utenti devono comunque essere in grado di eseguire l'accesso.

Client desktop remoto sta tentando di connettersi anche rispondere.

Per risolvere questo problema, riavviare il server host sessione Desktop remoto.

Per risolvere questo problema, si applicano 4093114 KB, [10 aprile 2018, ovvero KB4093114 (aggiornamento cumulativo mensile)] (file:///C:\\Users\\v-jesits\\AppData\\Local\\Microsoft\\Windows\\INetCache\\Content.Outlook\\FUB8OO45\\April%2010,%202018—KB4093114%20\(Monthly% 20Rollup\)), per i server host sessione Desktop remoto.

### Problema listener di desktop remoto

In alcuni server host sessione Desktop remoto che sono stati aggiornati direttamente da Windows Server 2008 R2 a Windows Server 2012 R2 o Windows Server 2016 è stato rilevato un problema. Quando un client desktop remoto si connette al server host sessione Desktop remoto, il server host sessione Desktop remoto crea un listener di desktop remoto per la sessione dell'utente. I server interessati mantenere un conteggio dei listener di desktop remoto che aumenta man mano che gli utenti si connettono, ma mai riduzioni.

Per risolvere questo problema, è possibile utilizzare i metodi seguenti:

  - Riavviare il server host sessione Desktop remoto per reimpostare il conteggio del listener di desktop remoto
  - Modificare i criteri di limite di connessione, impostata su un valore di grandi dimensioni. Per ulteriori informazioni sulla gestione dei criteri di limite di connessione, vedere [controllare i criteri di limite di connessione](#check-the-connection-limit-policy).

Per risolvere questo problema, applicare gli aggiornamenti seguenti per i server host sessione Desktop remoto:

  - Windows Server 2012 R2: KB, 4343891 [30 agosto 2018-KB4343891 (anteprima di cumulativo mensile)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB, 4343884 [30 agosto 2018-KB4343884 (Build del sistema operativo 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### Controllare i criteri di limite di connessione

È possibile impostare il limite al numero di connessioni desktop remote a livello di singolo computer o tramite la configurazione di un oggetto Criteri di gruppo (GPO). Per impostazione predefinita, il limite non è impostato.

Per controllare le impostazioni correnti e identificare eventuali oggetti Criteri di gruppo esistente nel server host sessione Desktop remoto, Apri una finestra del prompt dei comandi come amministratore e immetti il comando seguente:
  
```
gpresult /H c:\gpresult.html
```
   
Al termine di questo comando, Apri gpresult.html e in **Configurazione computer\\modelli amministrativi\\componenti di Components\\Remote Desktop Services\\Remote sessione Desktop Host\\Connections**, trova il **Limita il numero di connessioni** criteri.

  - Se l'impostazione di questo criterio è **disabilitato**, criteri di gruppo sono non limitare le connessioni RDP.
  - Se l'impostazione di questo criterio è **abilitato**, quindi controlla **Vince oggetto Criteri di gruppo**. Se devi rimuovere o modificare il limite di connessione, modificare questo oggetto Criteri di gruppo.

Per applicare le modifiche dei criteri, Apri una finestra del prompt dei comandi nel computer interessato e immetti il comando seguente:
  
```
gpupdate /force
```
  
## Client desktop remoto disconnette e non è possibile riconnettere alla stessa sessione

Dopo che i client desktop remoto perde il collegamento sul desktop remoto, il client non possa riconnettersi immediatamente. L'utente riceve messaggi di errore simile al seguente:

  - A causa di un errore di sicurezza, il client Impossibile connettersi al server Terminal. Dopo avere verificato che sono connessi alla rete, riprovare a connettersi al server.
  - Desktop remoto disconnesso. A causa di un errore di sicurezza, il client Impossibile connettersi al computer remoto. Verifica che si sono connessi alla rete e riprovare.

In un secondo momento, il client desktop remoto riconnetterà a desktop remoto. Tuttavia, invece che connette il client alla sessione originale, il server host sessione Desktop remoto connette il client a una nuova sessione. Il controllo del server host sessione Desktop remoto rivela che la sessione originale non è stato immesso stato disconnesso, ma invece rimane attivo.

Per risolvere questo problema, puoi abilitare i criteri di **intervallo di connessione keep-alive Configura** nella configurazione computer\\modelli amministrativi\\componenti di Components\\Remote Desktop Services\\Remote sessione Desktop Host\\Connections ** **cartella dei criteri di gruppo. Se si abilita questo criterio, è necessario immettere un intervallo keep-alive. L'intervallo keep-alive determina la frequenza, in minuti, il server controlla lo stato della sessione.

Questo problema può essere causato da una configurazione errata delle impostazioni di configurazione e autenticazione. È possibile configurare queste impostazioni a livello di server o usando oggetti Criteri di gruppo. Le impostazioni di criteri di gruppo sono disponibili nella cartella **Configurazione computer\\modelli amministrativi\\componenti di Components\\Remote Desktop Services\\Remote sessione Desktop Host\\Security** criteri di gruppo.

1. Nel server Host sessione Desktop remoto, Apri configurazione Host sessione Desktop remoto.
2. Sotto **le connessioni**, destro del mouse sul nome della connessione e quindi fai clic su **proprietà**.
3. Nella finestra di dialogo **proprietà** per la connessione, nella scheda **Generale** , il livello di **sicurezza** , seleziona un metodo di protezione.
4. **Livello di crittografia**, selezionare il livello che vuoi. È possibile selezionare **bassa**, **Client compatibili**, **elevato**o **Conforme a FIPS**.

> [!NOTE]  
>  - Quando le comunicazioni tra client e server Host sessione Desktop remoto richiedono il massimo livello di crittografia, Usa la crittografia FIPS compatibile.
>  - Eventuali impostazioni a livello di crittografia che configurare in Criteri di gruppo sostituiscono la configurazione che impostato tramite lo strumento di configurazione di Servizi Desktop remoto. Inoltre, se si abilita la [crittografia di sistema: algoritmi Usa FIPS compatibili per crittografia, hash e firma](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)) i criteri di **impostare il livello di crittografia connessione client** esegue l'override di criteri, questa impostazione. I criteri di crittografia di sistema sono nella cartella **Computer Configuration\\Windows Settings\\Security Settings\\Impostazioni Policies\\Security opzioni** .
>  - Quando modifichi il livello di crittografia, il nuovo livello di crittografia ha effetto al successivo che accesso dell'utente. Se sono necessari più livelli di crittografia in un unico server, installare più schede di rete e configurare ogni scheda separatamente.
>  - Per verificare che il certificato è una chiave privata corrispondente, nella configurazione di Servizi Desktop remoto, la connessione per cui vuoi visualizzare il certificato, selezionare **Generale**, selezionare **Modifica**, seleziona il certificato che vuoi destro visualizzare e quindi seleziona **Il certificato di visualizzazione**. Nella parte inferiore della scheda **Generale** , l'istruzione "disponi di una chiave privata che corrisponde a questo certificato" dovrebbe essere visualizzata. È inoltre possibile visualizzare queste informazioni utilizzando lo snap-in certificati.
>  - Crittografia FIPS (il **crittografia di sistema: algoritmi Usa FIPS compatibili per crittografia, hash e firma** criteri o impostazione **FIPS compatibile** nella configurazione del Server di Desktop remoto) consente di crittografare e decrittografare i dati inviati dal client al server e dal server al client, con gli algoritmi di crittografia 140-1 Federal Information Processing Standard (FIPS), utilizzando i moduli di crittografia Microsoft. Per altre informazioni, vedi [FIPS 140 convalida](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation).
>  - L'impostazione **elevato** crittografa i dati inviati dal client al server e dal server al client con crittografia a 128 bit.
>  - L'impostazione **Client compatibili** crittografa i dati inviati tra il client e il server di forza della chiave massima supportata dal client.
>  - L'impostazione di **condizioni di scarsa** crittografa i dati inviati dal client al server tramite la crittografia 56 bit.

## Remoto portatile si scollega dalla rete wireless

Questo problema può verificarsi quando un client desktop remoto si connette a un computer portatile tramite una rete wireless 802.1 x. Il portatile in modo intermittente, Scollega dalla rete wireless e non riconnettersi automaticamente.

Questo è un problema noto che si verifica quando l'impostazione di autenticazione di rete per la connessione di rete wireless è **l'autenticazione utente**.

Per risolvere questo problema, impostare l'impostazione di autenticazione di rete di **Computer**o **l'autenticazione utente o computer** .

 > [!NOTE]  
> Per modificare le impostazioni di autenticazione di rete in un singolo computer, devi usare il pannello di controllo Centro rete e condivisione per creare una nuova connessione wireless con le nuove impostazioni.

Per una descrizione completa di come configurare le impostazioni di rete wireless con oggetti Criteri di gruppo, vedi [i criteri di configurare la rete Wireless (IEEE 802.11)](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## L'esperienza degli utenti prestazioni scarse o problemi di applicazione

Questa sezione consente di risolvere diversi problemi comuni che gli utenti potrebbero riscontrare quando si utilizza la funzionalità desktop remota:

  - [Gli utenti connessi in nuove macchine virtuali Microsoft Azure in modo intermittente riscontrare problemi](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Gli utenti connessi in Desktop remoto di Windows 10 versione 1709 potrebbero verificarsi problemi la riproduzione di video](#video-playback-issues-on-windows-10-version-1709).
  - [Gli utenti con i profili utente di sola lettura che si connettono a Windows 10 desktop remoto non possono condividere loro desktop.](#desktop-sharing-issues-on-windows-10)
  - [Utenti che si connettono a Windows 10 desktop remoto con i client che eseguono in versioni precedenti di Windows 10 prestazioni scarse quando NLA è disabilitato](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Quando gli utenti di eseguono più applicazioni in sessioni desktop remoto e scollegare e ricollegare frequentemente, si possono verificarsi una schermata nera sulla riconnessione.](#black-screen-issue)

### Problemi occasionali con nuove macchine virtuali di Microsoft Azure

Questo problema riguarda le macchine virtuali che è stato effettuato il di recente. Dopo che l'utente si connette alla macchina virtuale, la sessione desktop remoto non può caricare le impostazioni dell'utente in modo corretto.

Per risolvere questo problema, Disconnetti dalla macchina virtuale e quindi connettersi nuovamente attendere almeno 20 minuti.

Per risolvere questo problema, applicare gli aggiornamenti seguenti per le macchine virtuali, come appropriato:

  - Windows 10 e Windows Server 2016: KB, 4343884 [30 agosto 2018-KB4343884 (Build del sistema operativo 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB, 4343891 [30 agosto 2018-KB4343891 (anteprima di cumulativo mensile)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### Problemi di riproduzione di video in Windows 10 versione 1709

Questo problema si verifica quando gli utenti si connettono ai computer remoti che eseguono Windows 10, versione 1709. Quando questi utenti Riproduci video usando la VMR9 codec (Video MixingRenderer9), il giocatore Mostra solo una finestra nera.

Questo è un problema noto in Windows 10, versione 1709. Il problema non si verifica in Windows 10 versione 1703.

### Condivisione problemi in Windows 10 desktop

Questo problema si verifica quando l'utente ha un profilo utente di sola lettura e hive del Registro di sistema, ad esempio in uno scenario di modalità tutto schermo. Quando un utente si connette a un computer remoto che esegue Windows 10, versione 1803, non possono condividere il desktop.

Per risolvere questo problema, applicare l'aggiornamento di Windows 10, 4340917 [24 luglio 2018-KB4340917 (17134.191 Build del sistema operativo)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### Problemi di prestazioni quando si unisce le versioni di Windows 10 se NLA è disabilitato

Questo problema si verifica quando NLA è disabilitato e i computer client desktop remoto che eseguono Windows 10 si connettono ai desktop remoti che eseguono versioni diverse di Windows 10. Gli utenti dei client desktop remoto nei computer che eseguono Windows 10, versione 1709 o versioni precedenti riscontrare prestazioni scarse quando si connettono a desktop remoti che eseguono Windows 10, versione 1803 o successiva.

Ciò si verifica perché, quando NLA è disabilitato, i computer client precedenti usano un protocollo più lento quando si connettono a Windows 10, versione 1803 o versione successiva.

Per risolvere questo problema, si applicano 4340917 KB, [24 luglio 2018-KB4340917 (17134.191 Build del sistema operativo)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### Schermata nera

Questo problema si verifica in Windows 8.0, Windows 8.1, Windows 10 RTM e Windows Server 2012 R2. Un utente avvia un desktop remoto di più applicazioni, quindi si scollega dalla sessione. Periodicamente, l'utente si riconnetterà a desktop remoto a interagire con le applicazioni e quindi disconnettersi nuovamente. A un certo punto, quando l'utente si riconnette, la sessione desktop remoto Mostra solo una schermata nera. L'utente deve terminare la sessione desktop remoto dalla console del computer remoto (o la console di server host sessione Desktop remoto). Questa azione arresta le applicazioni.

Per risolvere questo problema, si applicano gli aggiornamenti seguenti come appropriato:

  - Windows 8 e Windows Server 2012: KB4103719, [17 maggio 2018-KB4103719 (anteprima di cumulativo mensile)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 e Windows Server 2012 R2: KB4103724, [17 maggio 2018-KB4103724 (anteprima di cumulativo mensile)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) e KB, 4284863 [21 giugno 2018, ovvero KB4284863 (anteprima di cumulativo mensile)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10: Corretto in KB4284860, [12 giugno 2018, ovvero KB4284860 (Build del sistema operativo 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
