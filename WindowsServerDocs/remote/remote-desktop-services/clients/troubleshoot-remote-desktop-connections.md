---
title: Risoluzione dei problemi relativi a connessioni Desktop remoto
description: Risoluzione dei problemi relativi a procedure organizzate per sintomo
ms.custom: na
ms.reviewer: rklemen; josh.bender
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: RobVyK
manager: ''
ms.author: kaushika; rklemen; josh.bender; v-tea
ms.date: 02/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: 43e40f8442600dfc66dafd6b8b210274908b4595
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446727"
---
# <a name="troubleshooting-remote-desktop-connections"></a>Risoluzione dei problemi relativi a connessioni Desktop remoto
Per una breve spiegazione di alcuni tra i problemi più comuni di Servizi Desktop remoto (RDS), vedere [domande frequenti su client di Desktop remoto](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). Questo articolo descrive vari approcci più avanzati per la risoluzione dei problemi di connessione. Molte di queste procedure si applicano se la risoluzione di una semplice configurazione, ad esempio un computer fisico, la connessione a un altro computer fisico o una configurazione più complessa. Alcune procedure di risolvono i problemi che si verificano solo in scenari più complessi con più utenti. Per altre informazioni sui componenti di desktop remoto e come interagiscono tra loro, vedere [architettura di Servizi Desktop remoto](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> Molte delle procedure descritte in questo articolo è necessario per accedere a più computer, che alcune delle quali potrebbe essere necessario accedere in remoto. Per altre informazioni sugli strumenti di amministrazione remota e come configurarle, vedere [amministrazione remota del Server strumenti () per i sistemi operativi Windows](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

Nell'elenco seguente, identificare il tipo del sintomo cui si sono verificati è (o gli utenti).

- [Il client desktop remoto non è possibile connettersi a desktop remoto, ma non sono presenti i sintomi specifici o messaggi (risoluzione dei problemi generali)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [Il client desktop remoto non è possibile connettersi a desktop remoto e riceve un messaggio di "Classe non registrata"](#client-cannot-connect-class-not-registered)
- [Il client desktop remoto non è possibile connettersi a desktop remoto e non riceve una "sono disponibili licenze" o "errore di sicurezza" messaggio](#client-cannot-connect-no-licenses-available)
- [L'utente riceve un messaggio "Accesso negato" oppure deve fornire le credenziali due volte](#user-cannot-authenticate-or-must-authenticate-twice)
- [Sulla connessione, il messaggio di "Servizi Desktop remoto sono attualmente occupato"](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [Il client desktop remoto si disconnette e non può riconnettersi alla stessa sessione](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [L'utente si connette a un computer portatile in remoto su una rete wireless e quindi sul computer portatile si disconnette dalla rete](#remote-laptop-disconnects-from-wireless-network).
- [L'utente si verifichi una riduzione delle prestazioni o problemi relativi ad applicazioni remote](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Per un elenco aggiornato dei codici disconnect RDP, vedere [enumerazione ExtendedDisconnectReasonCode](https://docs.microsoft.com/en-us/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>Nessun sintomi specifici o messaggi (risoluzione dei problemi generali)

Usare questi passaggi quando un client Desktop remoto non è possibile connettersi a un desktop remoto, ma non fornisce i messaggi o altri sintomi che contribuiscono a identificano la causa. Per risolvere molti delle cause più comuni di questo tipo di problema, usare i metodi seguenti:

- [Controllare lo stato del protocollo RDP](#check-the-status-of-the-rdp-protocol)
- [Controllare lo stato dei servizi RDP](#check-the-status-of-the-rdp-services)
- [Verificare che il listener RDP è corretto](#check-that-the-rdp-listener-is-functioning)
- [Verificare la porta del listener RDP](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>Controllare lo stato del protocollo RDP

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Controllare lo stato del protocollo RDP in un computer locale

Per controllare e modificare lo stato del protocollo RDP in un computer locale, vedere [come abilitare Desktop remoto](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Se le opzioni desktop remote non sono disponibili, vedere [controlla se un oggetto Criteri di gruppo sta bloccando il protocollo RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Controllare lo stato del protocollo RDP in un computer remoto

> [!IMPORTANT]  
> Seguire con attenzione i passaggi descritti in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [eseguire il backup Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verificano problemi.

Per controllare e modificare lo stato del protocollo RDP in un computer remoto, usare una connessione di rete del Registro di sistema:

1. Selezionare **avviare**, selezionare **eseguito**, quindi immettere **regedt32**.
2. Nell'Editor del Registro di sistema, selezionare **File**, quindi selezionare **connettersi registro rete**.
3. Nel **seleziona Computer** finestra di dialogo casella, immettere il nome del computer remoto, selezionare **Controlla nomi**, quindi selezionare **OK**.
4. Passare a **HKEY\_locale\_MACHINE\\SYSTEM\\CurrentControlSet\\controllo\\Server Terminal**.  
   ![Editor del Registro di sistema, che mostra la voce del valore fDenyTSConnections](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - Se il valore della **valore fDenyTSConnections** ritenuto **0**, quindi RDP è abilitato
   - Se il valore della **valore fDenyTSConnections** ritenuto **1**, RDP è disabilitata
5. Per abilitare desktop remoto, modificare il valore della **valore fDenyTSConnections** dalla **1** al **0**.

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Controllare se un oggetto Criteri di gruppo (GPO) sta bloccando il protocollo RDP in un computer locale

Se è possibile attivare il protocollo RDP nell'interfaccia utente o se il valore di **valore fDenyTSConnections** ripristinata **1** dopo che è stata modificata, un oggetto Criteri di gruppo potrebbe essere viene sottoposto a override le impostazioni a livello di computer.

Per controllare la configurazione dei criteri di gruppo in un computer locale, aprire una finestra del prompt dei comandi come amministratore e immettere il comando seguente:
```
gpresult /H c:\gpresult.html
```
Al termine di questo comando, aprire gpresult.html. Nelle **configurazione Computer\\modelli amministrativi\\i componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\connessioni**, trovare il **consentire agli utenti di connettersi in remoto tramite Remote Desktop Services** criteri.

- Se è l'impostazione di questo criterio **abilitato**, criteri di gruppo non blocca le connessioni RDP.
- Se è l'impostazione di questo criterio **disabilitati**, controllare **dominanti GPO**. Questo è l'oggetto Criteri di gruppo sta bloccando le connessioni RDP.
  ![Un segmento di esempio di gpresult.html, in cui l'oggetto Criteri di gruppo a livello di dominio * * Blocca RDP * * è la disabilitazione di RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Un segmento di esempio di gpresult.html, in cui * * locale gruppo criteri * * è la disabilitazione di RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Controllare se un oggetto Criteri di gruppo sta bloccando il protocollo RDP in un computer remoto

Per controllare la configurazione dei criteri di gruppo in un computer remoto, il comando è quasi uguale a quello di un computer locale:
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
Il file di questo comando produce (**gpresult -\<nome del computer\>HTML**) usa lo stesso formato di informazioni come la versione del computer locale (**gpresult.html**) viene utilizzato.

#### <a name="modifying-a-blocking-gpo"></a>Modifica di un oggetto Criteri di blocco

È possibile modificare queste impostazioni nel gruppo di criteri oggetto Editor (GPE) e gruppo di criteri di Management Console (GPM). Per altre informazioni su come usare criteri di gruppo, vedere [Advanced Group Policy Management](https://docs.microsoft.com/en-us/microsoft-desktop-optimization-pack/agpm/).

Per modificare il criterio di blocco, usare uno dei metodi seguenti:

- In GPE, accedere a livello di oggetto Criteri di gruppo appropriato (ad esempio, locale o dominio) e passare a **configurazione Computer\\modelli amministrativi\\i componenti di Windows\\Remote Desktop Services\\ Host sessione Desktop remoto\\connessioni**\\**Consenti agli utenti di connettersi in remoto tramite Remote Desktop Services**.  
   1. Impostare il criterio **Enabled** oppure **non configurato**.
   2. Nei computer interessati, aprire una finestra del prompt dei comandi come amministratore ed eseguire la **gpupdate /force** comando.
- In GPM, passare all'unità Organizzativa in cui viene applicato il criterio di blocco per i computer interessati ed eliminare i criteri dall'unità Organizzativa.

### <a name="check-the-status-of-the-rdp-services"></a>Controllare lo stato dei servizi RDP

Nel computer locale (client) e il computer remoto (destinazione), è necessario essere in esecuzione i servizi seguenti:

- Servizi Desktop remoto (TermService)
- Redirector porta UserMode di Servizi Desktop remoto (UmRdpService)

È possibile utilizzare lo snap-in servizi di MMC per gestire i servizi in locale o remoto. È anche possibile usare PowerShell in locale o remoto (se il computer remoto è configurato per accettare i comandi di PowerShell remoti).

![Servizi Desktop remoto nello snap-in MMC Servizi. Non modificare le impostazioni predefinite del servizio.](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

In dei computer, se uno o entrambi i servizi non sono in esecuzione, avviarli.

> [!NOTE]  
> Se si avvia il servizio Servizi Desktop remoto, fare clic su **Sì** per riavviare automaticamente il servizio Redirector porta di UserMode di Servizi Desktop remoto.

### <a name="check-that-the-rdp-listener-is-functioning"></a>Verificare che il listener RDP è corretto

> [!IMPORTANT]  
> Seguire con attenzione i passaggi descritti in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [eseguire il backup Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verificano problemi.

#### <a name="check-the-status-of-the-rdp-listener"></a>Controllare lo stato del listener del protocollo RDP

Per questa procedura, utilizzare un'istanza di PowerShell con autorizzazioni amministrative. Per un computer locale, è anche possibile usare un prompt dei comandi con autorizzazioni amministrative. Tuttavia, questa procedura Usa PowerShell perché gli stessi comandi funzionano sia in locale e in modalità remota.

1. Aprire una finestra di PowerShell. Per connettersi a un computer remoto, immettere **Enter-PSSession - ComputerName \<nome del computer\>** .
2. Immettere **qwinsta**. 
    ![Il comando qwinsta Elenca i processi in attesa su porte del computer.](../media/troubleshoot-remote-desktop-connections/WPS_qwinsta.png)
3. Se l'elenco include **rdp-tcp** con stato **ascolto**, funziona il listener RDP. Andare al [verificare la porta del listener RDP](#check-the-rdp-listener-port). In caso contrario, continuare al passaggio 4.
4. Esportare la configurazione del listener RDP da un computer di lavoro.
    1. Accedere a un computer con la stessa versione del sistema operativo come computer interessato e accedere Registro di sistema del computer (ad esempio, l'editor del Registro di sistema).
    2. Passare alla voce del Registro di sistema seguente:  
        **HKEY\_locale\_MACHINE\\SYSTEM\\CurrentControlSet\\controllo\\Server Terminal\\WinStations\\RDP-Tcp**
    3. Esportare la voce in un file con estensione reg. Ad esempio, nell'Editor del Registro di sistema, fare doppio clic la voce, selezionare **esportare**e quindi immettere un nome di file per le impostazioni esportate.
    4. Copiare il file con estensione reg esportato per il computer interessato.
5. Importare la configurazione del listener RDP, aprire una finestra di PowerShell con autorizzazioni amministrative nel computer interessato (o aprire la finestra di PowerShell e connettersi in remoto al computer interessati).
   1. Per eseguire il backup la voce del Registro di sistema esistente, immettere il comando seguente:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Per rimuovere la voce del Registro di sistema esistente, immettere i comandi seguenti:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Per importare la nuova voce del Registro di sistema e quindi riavviare il servizio, immettere i comandi seguenti:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      in cui \<filename\> è il nome del file con estensione reg esportato.
6. Testare la configurazione eseguendo nuovamente la connessione desktop remoto. Se non è ancora possibile connettersi, riavviare il computer interessato.
7. Se non è ancora possibile connettersi, [controllare lo stato del certificato autofirmato RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Controllare lo stato del certificato autofirmato RDP

1. Se non è ancora possibile connettersi, aprire lo snap-in MMC certificati. Quando viene chiesto di selezionare l'archivio certificati per gestire, selezionare **account del Computer**e quindi selezionare il computer interessato.
2. Nel **certificati** cartella sotto **Desktop remoto**, eliminare il certificato autofirmato RDP. 
    ![Certificati di Desktop remoti nello snap-in MMC certificati.](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. Nel computer interessato, riavviare il servizio Servizi Desktop remoto.
4. Aggiornare lo snap-in certificati.
5. Se non è stato ricreato, il certificato autofirmato RDP [controllare le autorizzazioni della cartella MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Controllare le autorizzazioni della cartella MachineKeys

1. Nel computer interessato, aprire Esplora e quindi passare al **c:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Fare doppio clic su **MachineKeys**, selezionare **delle proprietà**, selezionare **sicurezza**, quindi selezionare **avanzate**.
3. Assicurarsi che siano configurate le autorizzazioni seguenti:
      - Builtin\\amministratori: Controllo completo
      - Tutti gli utenti: Lettura, scrittura

### <a name="check-the-rdp-listener-port"></a>Verificare la porta del listener RDP

Nel computer locale (client) e il computer remoto (destinazione), il listener RDP deve essere in ascolto sulla porta 3389. Non altre applicazioni devono utilizzare questa porta.

> [!IMPORTANT]  
> Seguire con attenzione i passaggi descritti in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [eseguire il backup Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verificano problemi.

Per controllare o modificare la porta RDP, usare l'Editor del Registro di sistema:

1. Fare clic su Start, selezionare **eseguiti**, quindi immettere **regedt32**.
      - Per connettersi a un computer remoto, selezionare **File**, quindi selezionare **connettersi registro rete**.
      - Nel **seleziona Computer** finestra di dialogo casella, immettere il nome del computer remoto, selezionare **Controlla nomi**, quindi selezionare **OK**.
2. Aprire il Registro di sistema e spostarsi **HKEY\_locale\_MACHINE\\SYSTEM\\CurrentControlSet\\controllo\\Terminal Server\\WinStations\\ \<listener\>** . 
    ![La sottochiave del numero di porta per il protocollo RDP.](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. Se **NumeroPorta** ha un valore diverso da **3389**, modificarlo in base ai **3389**. 
   > [!IMPORTANT]  
    > È possibile utilizzare Servizi Desktop remoto con un'altra porta. Tuttavia, non è consigliabile farlo. Risoluzione dei problemi relativi a questa configurazione non rientra nell'ambito di questo articolo.
4. Dopo aver modificato il numero di porta, riavviare il servizio Servizi Desktop remoto.

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>Verificare che un'altra applicazione non tenta di utilizzare la stessa porta

Per questa procedura, utilizzare un'istanza di PowerShell con autorizzazioni amministrative. Per un computer locale, è anche possibile usare un prompt dei comandi con autorizzazioni amministrative. Tuttavia, questa procedura Usa PowerShell in quanto gli stessi comandi funzionano in modalità locale e remota.

1. Aprire una finestra di PowerShell. Per connettersi a un computer remoto, immettere **Enter-PSSession - ComputerName \<nome del computer\>** .
2. Immettere il comando seguente:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![Il comando netstat produce un elenco di porte e i servizi in ascolto su tali.](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. Cercare una voce per la porta TCP 3389 (o la porta assegnata a RDP) con lo stato **In ascolto**. 
    > [!NOTE]  
   > Il PID del processo o del servizio che utilizza la porta è visualizzato nella colonna PID.
4. Per determinare l'applicazione che utilizza la porta 3389 (o la porta assegnata a RDP), immettere il comando seguente:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![Il comando di tasklist riporta i dettagli di un processo specifico.](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. Cercare una voce per il numero PID associato con la porta (dal **netstat** output). I servizi o i processi che sono associati a tale PID visualizzati sulla destra.
6. Se un'applicazione o un servizio diverso da Servizi Desktop remoto (TermServ.exe) utilizza la porta, è possibile risolvere il conflitto usando uno dei metodi seguenti:
      - Configurare l'altra applicazione o servizio per usare una porta diversa (scelta consigliata).
      - Disinstallare l'altra applicazione o servizio.
      - Configurare il protocollo RDP per l'uso di una porta diversa e quindi riavviare il servizio Servizi Desktop remoto (non consigliato).

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Controllare se un firewall sta bloccando la porta RDP

Usare la **psping** lo strumento per testare se è possibile raggiungere il computer interessato utilizzando la porta 3389.

1. In un computer diverso dal computer interessato, eseguire il download **psping** da <https://live.sysinternals.com/psping.exe>.
2. Aprire una finestra del prompt dei comandi come amministratore, passare alla directory in cui è installato **psping**, quindi immettere il comando seguente:  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Controllare l'output del **psping** comando per ottenere risultati simile al seguente:  
      - **La connessione al \<IP di computer\>** : Il computer remoto è raggiungibile.
      - **(0% loss)** : Tutti i tentativi di connessione ha avuto esito positivo.
      - **Il computer remoto ha rifiutato la connessione di rete**: Il computer remoto non è raggiungibile.
      - **(100% loss)** : Tutti i tentativi di connessione non riuscita.
1. Eseguire **psping** in più computer per testare la possibilità di connettersi al computer interessato.
1. Si noti come se il computer interessato blocca le connessioni da tutti gli altri computer, alcuni altri computer o da un solo altro computer.
1. Passaggi successivi consigliati:
      - Coinvolgi gli amministratori di rete per verificare che la rete consenta il traffico RDP al computer interessato.
      - Esaminare le configurazioni di eventuali firewall tra il computer di origine e il computer interessato (incluso Windows Firewall nel computer interessato) per determinare se un firewall sta bloccando la porta RDP.

## <a name="client-cannot-connect-class-not-registered"></a>Client non può connettersi, "Classe non è registrato"

Quando si prova a connettersi a un computer remoto usando un client che esegue Windows 10, versione 1709 o versioni successive, il client potrebbe non connettersi mentre il server Host sessione Desktop remoto indica un messaggio che contiene il codice di errore "Classe non registrata (0x80040154)".

Questo problema si verifica se l'utente che sta tentando di connettersi ha un profilo utente obbligatorio. Per risolvere questo problema, installare il [24 luglio 2018, ovvero KB4338817 (16299.579 di Build del sistema operativo)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817) aggiornamento di Windows 10.

## <a name="client-cannot-connect-no-licenses-available"></a>Client non può connettersi, non sono disponibili licenze

Questa situazione si applica alle distribuzioni che includono un server host sessione Desktop remoto e un server licenze Desktop remoto.

Identificare il tipo di comportamento vengono visualizzati gli utenti:

- La sessione è stata disconnessa perché non ci sono licenze disponibili o non è disponibile alcun server licenze
- Accesso negato a causa di un errore di sicurezza

Accedere all'Host sessione Desktop remoto come amministratore di dominio e aprire la strumento di diagnosi di licenze di desktop remoto. Cercare messaggi simili ai seguenti:

  - Il periodo di tolleranza per il controllo remoto è scaduto il server Host sessione desktop, ma il server Host sessione Desktop remoto non è stato configurato con tutti i server licenze. le connessioni al server Host sessione Desktop remoto verranno rifiutate a meno che non è configurato un server licenze per il server Host sessione Desktop remoto.
  - Server licenze \<nome computer\> non è disponibile. Potrebbe essere causato da problemi di connettività di rete, il servizio licenze Desktop remoto è arrestato nel server licenze o servizio licenze Desktop remoto non viene più installata nel computer.

Questi problemi tendono a essere associati con i messaggi utente seguenti:

  - La sessione remota è stata disconnessa perché non sono disponibili per questo computer non licenze di accesso client di Desktop remoto.
  - La sessione remota è stata disconnessa perché non esistono Nessun server di licenze Desktop remoto disponibili per fornire una licenza.

In questo caso [configurare il servizio licenze Desktop remoto](#configure-the-rd-licensing-service).

Se di strumento di diagnosi di licenze di desktop remoto sono elencati altri problemi, ad esempio "il componente del protocollo RDP X.224 ha rilevato un errore nel flusso del protocollo e ha disconnesso il client," potrebbe esistere un problema che interessa i certificati di licenze. Tali problemi tendono a essere associato ai messaggi utente, ad esempio:

A causa di un errore di sicurezza, il client non è stato possibile connettersi al server Terminal. Dopo aver verificato che si è connessi alla rete, provare a riconnettersi al server.

In questo caso [chiavi del Registro di sistema del certificato X509 aggiornamento](#refresh-the-x509-certificate-registry-keys).

### <a name="configure-the-rd-licensing-service"></a>Configurare il servizio licenze Desktop remoto

La procedura seguente usa Server Manager per apportare le modifiche di configurazione. Per informazioni su come configurare e usare Server Manager, vedere [Server Manager](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager).

1. Aprire Server Manager e quindi passare al **Servizi Desktop remoto**.
2. Sul **Cenni preliminari sulla distribuzione**, selezionare **attività**, quindi selezionare **modificare le proprietà di distribuzione**.
3. Selezionare **servizio licenze Desktop remoto**, quindi selezionare la modalità di gestione delle licenze appropriata per la distribuzione (**per ogni dispositivo** oppure **per ogni utente**).
4. Immettere il nome di dominio completo (FQDN) del server licenze Desktop remoto e quindi selezionare **Add**.
5. Se si dispone di più di un server licenze Desktop remoto, ripetere il passaggio 4 per ogni server. 
    ![Opzioni di configurazione di server licenze Desktop remoto in Server Manager.](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>Le chiavi del Registro di sistema di aggiornamento X509 certificato

> [!IMPORTANT]  
> Seguire con attenzione i passaggi descritti in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [eseguire il backup Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verificano problemi.

Per risolvere questo problema, eseguire il backup e chiavi del Registro di sistema del certificato X509 remove, riavviare il computer, quindi server di gestione licenze reactivate del desktop remoto. Per farlo, eseguire le seguenti operazioni:

> [!NOTE]  
> Eseguire la procedura seguente in ogni server host sessione Desktop remoto.

1. Aprire l'Editor del Registro di sistema e spostarsi **HKEY\_locale\_MACHINE\\SYSTEM\\CurrentControlSet\\controllo\\Terminal Server\\RCM**.
2. Nel menu del Registro di sistema, selezionare **Esporta File del Registro di sistema**.
3. Tipo di **certificato esportati** nel **nome File** e quindi selezionare **Salva**.
4. Fare doppio clic su ognuno dei seguenti valori, selezionare **eliminare**, quindi selezionare **Yes** per verificare l'eliminazione:  
      - **Certificato**
      - **Certificato X509**
      - **ID Certificato X509**
      - **X509 Certificate2**
5. Uscire dall'Editor del Registro di sistema e quindi riavviare il server host sessione Desktop remoto.

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>Utente non riesce ad autenticare o deve eseguire l'autenticazione due volte

Esistono diversi problemi che possono causare problemi che interessano l'autenticazione dell'utente. Questa sezione vengono analizzati i casi seguenti:

  - [Un utente viene negato l'accesso con un messaggio "tipo di accesso limitato"](#access-denied-restricted-type-of-logon)
  - [Un utente o un'applicazione viene negata l'accesso con evento 16965, "una chiamata remota per il database SAM è stata negata"](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Un utente non è possibile accedere usando una smart card](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Se il computer remoto è bloccato, un utente deve immettere una password due volte](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Un utente non può accedere e riceve messaggi "Monitoraggio e aggiornamento oracle crittografia di CredSSP" e "errore di autenticazione"](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Dopo aver aggiornato i computer client, alcuni utenti è necessario eseguire l'accesso due volte](#after-you-update-client-computers-some-users-need-to-sign-in-twice).
  - [Gli utenti vengono negati l'accesso in una distribuzione che usa RemoteGuard con più gestori connessione desktop remoto](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>Accesso negato, tipo con restrizioni di accesso

In questo caso, un utente di Windows 10 sta tentando di connettersi ai computer Windows 10 o Windows Server 2016 viene negato l'accesso con il messaggio seguente:

> Connessione Desktop remoto:  
> L'amministratore di sistema ha limitato il tipo di accesso (rete o interattiva) possono essere utilizzate. Per ottenere assistenza, contattare l'amministratore di sistema o il supporto tecnico.

Questo problema si verifica quando l'autenticazione a livello di rete (NLA) è obbligatorio per le connessioni RDP e l'utente non è un membro del **Remote Desktop Users** gruppo. Può verificarsi anche se il **Remote Desktop Users** gruppo non è stato assegnato per il **Accedi al computer dalla rete** diritto utente.

Per risolvere questo problema, attenersi a uno di questi passaggi:

  - [Modificare l'appartenenza al gruppo dell'utente o Assegnazione diritti utente](#modify-the-users-group-membership-or-user-rights-assignment).
  - Disattivare NLA (scelta non consigliata).
  - Usare il client desktop remoto diverso da Windows 10. Ad esempio, i client Windows 7 non è il problema.

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modificare l'appartenenza al gruppo dell'utente o Assegnazione diritti utente

Se si verifica questo problema, in un singolo utente, la soluzione più semplice a questo problema consiste nell'aggiungere l'utente per il **Remote Desktop Users** gruppo.

Se l'utente è già un membro di questo gruppo (o se più i membri del gruppo hanno lo stesso problema), controllare la configurazione dei diritti di utente nel computer remoto Windows 10 o Windows Server 2016.

1. Aprire gruppo criteri oggetto Editor (GPE) e connettersi a criteri locali del computer remoto.
2. Passare a **configurazione Computer\\delle impostazioni di Windows\\Impostazioni sicurezza\\criteri locali\\Assegnazione diritti utente**, fare doppio clic su **accedere a questa computer dalla rete**, quindi selezionare **proprietà**.
3. Controllare l'elenco di utenti e gruppi **Remote Desktop Users** (o un gruppo padre).
4. Se l'elenco non include **Remote Desktop Users** (o un elemento padre del gruppo, ad esempio **tutti gli utenti**) è necessario aggiungerlo all'elenco (se sono presenti più di uno o due computer nella distribuzione, usare un oggetto Criteri di gruppo) .  
    Ad esempio, l'appartenenza predefinita per **Accedi al computer dalla rete** include **Everyone**. Se la distribuzione Usa un oggetto Criteri di gruppo da rimuovere **Everyone**, potrebbe essere necessario ripristinare l'accesso, aggiornare l'oggetto Criteri di gruppo per aggiungere **Remote Desktop Users**.

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Accesso negato, una chiamata remota per il database SAM è stata negata.

Questo comportamento è più probabile che si verificano se i controller di dominio eseguono Windows Server 2016 o versione successiva e gli utenti tentano di connettersi tramite un'app di connessione personalizzata. In particolare, le applicazioni che accedono a informazioni sul profilo dell'utente in Active Directory verranno negate l'accesso.

Questo comportamento è dovuta a una modifica a Windows. In Windows Server 2012 R2 e versioni precedenti versioni, quando un utente accede a un desktop, contatti di Manager di connessione remota (RCM) remoto il controller di dominio (DC) per eseguire una query le configurazioni specifiche di Desktop remoto nell'oggetto utente nel dominio di Active Directory Servizi (AD DS). Queste informazioni vengono visualizzate nella scheda profilo Servizi Desktop remoto di proprietà dell'oggetto dell'utente in Active Directory Users e snap-in MMC di computer.

A partire da Windows Server 2016, non è più una RCM query all'oggetto utenti in Active Directory Domain Services. Se è necessario RCM eseguire una query Active Directory Domain Services poiché si siano utilizzando gli attributi di Servizi Desktop remoto, è necessario abilitare manualmente il comportamento precedente RCM.

> [!IMPORTANT]  
> Seguire con attenzione i passaggi descritti in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [eseguire il backup Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verificano problemi.

Per abilitare il comportamento RCM legacy in un server Host sessione Desktop remoto, configurare le voci del Registro di sistema seguente e quindi riavviare la **Servizi Desktop remoto** servizio:  
  - **HKEY\_locale\_MACHINE\\SOFTWARE\\criteri\\Microsoft\\Windows NT\\servizi Terminal**
  - **HKEY\_locale\_MACHINE\\SYSTEM\\CurrentControlSet\\controllo\\Terminal Server\\WinStations\\\<nome workstation\>\\**  
      - Name: **fQueryUserConfigFromDC**
      - Digitare:  **Reg\_DWORD**
      - Valore: **1** (decimale)

Per abilitare il comportamento RCM legacy in un server diverso da un server Host sessione Desktop remoto, configurare queste voci del Registro di sistema e la seguente voce del Registro di sistema aggiuntive (e quindi riavviare il servizio):
  - **HKEY\_locale\_MACHINE\\SYSTEM\\CurrentControlSet\\controllo\\Terminal Server**

Per altre informazioni su questo comportamento, vedere KB 3200967 [cambia a Connection Manager remoto in Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>Un utente non è possibile accedere usando una smart card

Questo articolo vengono affrontati tre situazioni comuni in cui un utente non può accedere a un desktop remoto usando una smart card:  
  - [L'utente non può accedere a una succursale che utilizza un read-only Domain Controller (RODC)](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [L'utente non può accedere a un computer Windows Server 2008 SP2 che è stato aggiornato di recente](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [L'utente non è possibile rimanere connesso a un computer Windows che è stato aggiornato di recente e il servizio Servizi Desktop remoto diventa non risponde (blocchi)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>Non è possibile accedere con una smart card in una succursale con un controller di dominio

Questo problema si verifica nelle distribuzioni che includono un server host sessione Desktop remoto in un sito di succursale che utilizza un controller di dominio. Il server host sessione Desktop remoto si trova nel dominio radice. Gli utenti nel sito di succursale appartengono a un dominio figlio e utilizzano smart card per l'autenticazione. Il controller di dominio è configurato per memorizzare le password utente (il controller di dominio a cui appartiene il **consentiti replica passw**). Quando gli utenti tentano di accedere alle sessioni nel server host sessione Desktop remoto, riceve i messaggi, ad esempio"tentativo di accesso non valido. This is a causa di un nome utente o l'autenticazione di informazioni non validi".

Si tratta di un problema noto in modo che radice del controller di dominio e il controller di dominio di gestire la crittografia delle credenziali dell'utente. Radice del controller di dominio Usa una chiave di crittografia per crittografare le credenziali e il controller di dominio fornisce al client una chiave di decrittografia. Tuttavia, le due chiavi non corrispondono.

Per risolvere questo problema, provare a modificare la topologia di controller di dominio o disattivando la memorizzazione della password nel RODC o tramite la distribuzione di un controller di dominio scrivibile nel sito di succursale. Un metodo alternativo è spostare il server host sessione Desktop remoto allo stesso dominio figlio degli utenti. In alternativa è possibile consentire agli utenti di accedere senza smart card. Uno qualsiasi di queste soluzioni possono richiedere compromette le prestazioni o del livello di sicurezza.

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>Non è possibile accedere con una smart card per un computer Windows Server 2008 SP2

Questo problema si verifica quando l'utente accede a un computer Windows Server 2008 SP2 che è stato aggiornato con KB4093227 (2018.4B). Quando gli utenti tentano di accedere usando una smart card, accesso negato con i messaggi, ad esempio "Nessun certificato valido trovato. Verificare che la carta viene inserita correttamente e si adatta perfettamente." Allo stesso tempo, il computer Windows Server registra l'evento dell'applicazione "Errore durante il recupero di un certificato digitale dalla smart card inserita. Firma non valida."

Per risolvere questo problema, aggiornare il computer Windows Server con il 2018.06 B nuovo rilascio delle 4093227 KB, [descrizione dell'aggiornamento della sicurezza per la negazione di Windows Remote Desktop Protocol (RDP) di vulnerabilità nel servizio di Windows Server 2008: 10 aprile 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>Non è possibile restare connessi con una smart card e blocchi del servizio Servizi Desktop remoto

Questo problema si verifica quando l'utente accede a un computer Windows o Windows Server che è stato aggiornato con 4056446 KB. Inizialmente, l'utente potrebbe essere in grado di accedere al sistema usando una smart card, ma riceve quindi un "Rimuovi\_elettronica\_NO\_servizio" messaggio di errore. Il computer remoto potrebbe non rispondere.

Per risolvere questo problema, riavviare il computer remoto.

Per risolvere questo problema, aggiornare il computer remoto con la correzione appropriata:

  - Windows Server 2008 SP2: KB 4090928, [handle nel processo lsm.exe le perdite di Windows e applicazioni smart card possono visualizzare "Rimuovi\_elettronica\_NO\_servizio" errori](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB 4103724, [17 maggio 2018, ovvero KB4103724 (anteprima del Rollup mensile)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 e Windows 10, versione 1607: KB 4103720, [17 maggio 2018, ovvero KB4103720 (Build del sistema operativo 14393.2273)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>Se il computer remoto è bloccato, un utente deve immettere una password due volte

Questo problema può verificarsi quando un utente tenta di connettersi a un desktop remoto che esegue Windows 10 versione 1709 in una distribuzione in cui le connessioni RDP non richiedono NLA. In queste condizioni, se il desktop remoto è stato bloccato, l'utente deve immettere le proprie credenziali due volte durante la connessione.

Per risolvere questo problema, aggiornare il computer con Windows 10 versione 1709 con 4343893, KB [30 agosto 2018, ovvero KB4343893 (16299.637 di Build del sistema operativo)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>Utente non può accedere e riceve messaggi "Monitoraggio e aggiornamento oracle crittografia di CredSSP" e "errore di autenticazione"

Quando gli utenti tentano di accedere con qualsiasi versione di Windows da Windows Vista SP2 e versioni successive o Windows Server 2008 SP2 e versioni successive, cui viene negati l'accesso e ricevere messaggi simili ai seguenti:

  - Si è verificato un errore di autenticazione. La funzione richiesta non è supportata.
  - Ciò potrebbe dipendere da monitoraggio e aggiornamento di CredSSP crittografia oracle

"Correzione di oracle crittografia CredSSP" si riferisce a un set di aggiornamenti della sicurezza rilasciati nel mese di marzo, aprile e maggio 2018. CredSSP è un provider di autenticazione che elabora le richieste di autenticazione per le altre applicazioni. Il 13 marzo 2018, "3B" e gli aggiornamenti successivi indirizzato un exploit in cui un utente malintenzionato può inoltrare le credenziali utente per eseguire il codice nel sistema di destinazione.

Gli aggiornamenti iniziali aggiunto il supporto per un nuovo oggetto Criteri di gruppo **crittografia Oracle correzione**, che presenta le impostazioni possibili seguenti:

  - **Vulnerabile**. Le applicazioni client che utilizzano CredSSP possono ricorrere a versioni non sicure, ma questo comportamento espone il desktop remoto agli attacchi. Servizi che usano CredSSP accettano dei client che non sono stati aggiornati.
  - **Risolti**. Le applicazioni client che utilizzano CredSSP non è possibile eseguire il fallback alle versioni non sicure, ma i servizi che utilizzano CredSSP accettano dei client che non sono stati aggiornati.
  - **Forza aggiornamento client**. Le applicazioni client che utilizzano CredSSP non è possibile eseguire il fallback alle versioni non sicure e servizi che usano CredSSP non accetterà i client senza patch. 
    Nota: Questa impostazione non deve essere distribuita fino a quando tutti gli host remoti supportano la versione più recente.

L'aggiornamento all'8 maggio 2018, modificato il valore predefinito **crittografia Oracle correzione** impostazione dal **Vulnerable** al **mitigato**. Con questa modifica, i client desktop remoti con gli aggiornamenti non è possibile connettersi ai server che non sono (o aggiornato i server che non sono ancora stati riavviati). Per altre informazioni sugli effetti di aggiornamenti e i tipi di comunicazione che bloccano, vedere 4093492, KB [CredSSP Aggiorna per CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Per risolvere questo problema, assicurarsi che tutti i sistemi siano completamente aggiornati e riavviati. Per un elenco completo degli aggiornamenti e ulteriori informazioni sulla vulnerabilità, vedere [CVE-2018-0886 | Vulnerabilità di esecuzione di codice remoto CredSSP](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886).

Per risolvere questo problema finché non vengono completati gli aggiornamenti, controllare 4093492 KB per i tipi consentiti di connessioni. Se non esistono Nessun alternative fattibili si potrebbe prendere in considerazione uno dei metodi seguenti:

- Per i computer client interessati, impostare il **crittografia Oracle correzione** eseguire il criterio **Vulnerable**.
- Modificare i criteri seguenti nel **configurazione Computer\\modelli amministrativi\\i componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\ Sicurezza** cartella Criteri di gruppo:  
  - **Richiedono l'uso del livello di sicurezza specifici per le connessioni remote (RDP)** : impostato su **Enabled** e selezionare **RDP**.
  - **Richiedi autenticazione utente per le connessioni remote tramite autenticazione a livello di rete**: impostato su **disabilitato**.
    > [!IMPORTANT]  
    > Queste modifiche riducono la sicurezza della distribuzione. Devono solo essere temporanee, se si usa affatto.

Per altre informazioni sull'uso dei criteri di gruppo, vedere [modifica di un oggetto Criteri di blocco](#modifying-a-blocking-gpo).

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Dopo aver aggiornato i computer client, alcuni utenti è necessario eseguire l'accesso due volte

Gli utenti in una versione di Windows 7 o Windows 10 1709 accedere a una sessione desktop remoto e visualizzare immediatamente una seconda schermata di accesso. Questo problema si verifica se i computer client hanno ricevuto gli aggiornamenti seguenti:

  - Windows 7: KB 4103718, [all'8 maggio 2018, ovvero KB4103718 (aggiornamento cumulativo mensile)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB 4103727, [all'8 maggio 2018, ovvero KB4103727 (Build del sistema operativo 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Per risolvere questo problema, assicurarsi che i computer in cui gli utenti a cui connettersi (nonché i server host sessione Desktop remoto o RDVI) sono completamente aggiornati fino al giugno 2018. Ciò include gli aggiornamenti seguenti:

  - Windows Server 2016: KB 4284880, [12 giugno 2018, ovvero KB4284880 (Build del sistema operativo 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815, [12 giugno 2018, ovvero KB4284815 (aggiornamento cumulativo mensile)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855, [12 giugno 2018, ovvero KB4284855 (aggiornamento cumulativo mensile)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826, [12 giugno 2018, ovvero KB4284826 (aggiornamento cumulativo mensile)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564, [descrizione dell'aggiornamento della sicurezza per le vulnerabilità di esecuzione di codice remoto CredSSP in Windows Server 2008, Windows Embedded POSReady 2009 e 2009 Standard di Windows Embedded: 13 marzo 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>Gli utenti vengono negati l'accesso in una distribuzione che usa Remote Credential Guard con più gestori connessione desktop remoto

Questo problema si verifica nelle distribuzioni a disponibilità elevata che usano due o più gestori di connessione Desktop di remoto, se Windows Defender Remote Credential Guard è in uso. Gli utenti non possono accedere a desktop remoti.

Questo problema si verifica perché Remote Credential Guard usa Kerberos per l'autenticazione e limita NTLM. Tuttavia, in una configurazione a disponibilità elevata con bilanciamento del carico, i gestori connessione desktop remoto non può supportare operazioni di Kerberos.

Se è necessario usare una configurazione a disponibilità elevata con bilanciamento del carico di Gestore connessione desktop remoto, è possibile risolvere questo problema, disabilitare Credential Guard remoto. Per altre informazioni su come gestire Windows Defender Remote Credential Guard, vedere [credenziali di protezione Desktop remoto con Credential Guard remoto di Windows Defender](https://docs.microsoft.com/en-us/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Sulla connessione, l'utente riceve il messaggio "Servizio Desktop remoto è attualmente occupato"

Per determinare una risposta appropriata al problema, procedere come segue:

1. Il servizio Servizi Desktop remoto non risponde (ad esempio, il client desktop remoto viene visualizzato "bloccato" nella schermata iniziale).  
      - Se il servizio smette di rispondere, vedere [problema di memoria server host sessione Desktop remoto](#rdsh-server-memory-issue).
      - Se il client sembra essere l'interazione con il servizio in genere, continuare al passaggio successivo.
2. Se uno o più utenti disconnettono delle sessioni di desktop remote, gli utenti possono connettere nuovamente?  
   - Se il servizio continua a non consentire connessioni, indipendentemente dal numero di utenti disconnettere le sessioni, vedere [problema di listener di desktop remoto](#rd-listener-issue).
   - Se il servizio inizia ad accettare connessioni nuovamente dopo un numero di utenti hanno le sessioni disconnesse, [controllare i criteri di limite di connessione](#check-the-connection-limit-policy).

### <a name="rdsh-server-memory-issue"></a>Problema di memoria server host sessione Desktop remoto

È stata individuata una perdita di memoria su alcuni server host sessione Desktop remoto di Windows Server 2012 R2. Nel corso del tempo, questi server iniziano a rifiutare le connessioni desktop remoto sia accessi console locale con messaggi simili ai seguenti:

> L'attività che si sta tentando di eseguire operazioni non può essere completata perché il servizio Desktop remoto è attualmente occupato. Riprovare tra qualche minuto. Ad altri utenti devono comunque essere in grado di accedere.

Client desktop remoto tentando di connettersi anche smettere di rispondere.

Per risolvere questo problema, riavviare il server host sessione Desktop remoto.

Per risolvere questo problema, applicare 4093114 KB, [10 aprile 2018, ovvero KB4093114 (Rollup)](file:///C:/Users/v-jesits/AppData/Local/Microsoft/Windows/INetCache/Content.Outlook/FUB8OO45/April%2010,%202018—KB4093114%20(Monthly%20Rollup)) mensile, per il Server host sessione Desktop remoto.

### <a name="rd-listener-issue"></a>Problema di listener di desktop remoto

Un problema è stato notato su alcuni server host sessione Desktop remoto che sono stati aggiornati direttamente da Windows Server 2008 R2 a Windows Server 2012 R2 o Windows Server 2016. Quando un client desktop remoto si connette al server host sessione Desktop remoto, il server host sessione Desktop remoto crea un listener del desktop remoto per la sessione utente. Server interessati mantenere un conteggio di listener di desktop remoto che aumenta man mano che gli utenti si connettono, ma mai diminuisce.

Per risolvere questo problema, è possibile usare i metodi seguenti:

  - Riavviare il server host sessione Desktop remoto per reimpostare il conteggio dei listener di desktop remoto
  - Modificare i criteri di limite di connessione, impostandola su un valore molto grande. Per altre informazioni sulla gestione dei criteri di limite di connessione, vedere [controllare i criteri di limite di connessione](#check-the-connection-limit-policy).

Per risolvere questo problema, applicare gli aggiornamenti seguenti ai server host sessione Desktop remoto:

  - Windows Server 2012 R2: KB 4343891, [30 agosto 2018, ovvero KB4343891 (anteprima del Rollup mensile)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884, [30 agosto 2018, ovvero KB4343884 (Build del sistema operativo 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>Controllare i criteri di limite di connessione

È possibile impostare il limite sul numero di connessioni desktop remote simultanee a livello dei singoli computer o tramite la configurazione di un oggetto Criteri di gruppo (GPO). Per impostazione predefinita, il limite non viene impostato.

Per verificare le impostazioni correnti e identificare eventuali oggetti Criteri di gruppo esistente nel server host sessione Desktop remoto, aprire una finestra del prompt dei comandi come amministratore e immettere il comando seguente:
  
```
gpresult /H c:\gpresult.html
```
   
Al termine di questo comando, aprire gpresult.html e nel **configurazione Computer\\modelli amministrativi\\i componenti di Windows\\Remote Desktop Services\\Host sessione Desktop remoto \\Connessioni**, trovare il **Limita numero di connessioni** criteri.

  - Se è l'impostazione di questo criterio **disabilitato**, quindi criteri di gruppo non limitano le connessioni RDP.
  - Se è l'impostazione di questo criterio **Enabled**, quindi controllare **dominanti GPO**. Se è necessario rimuovere o modificare il limite di connessione, modificare questo oggetto Criteri di gruppo.

Per applicare le modifiche dei criteri, aprire una finestra del prompt dei comandi nel computer interessato e immettere il comando seguente:
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>Client desktop remoto si disconnette e non può riconnettersi alla stessa sessione

Dopo che i client desktop remoto perde la connessione desktop remoto, il client non può riconnettersi immediatamente. L'utente riceve i messaggi di errore simile al seguente:

  - A causa di un errore di sicurezza, il client non è stato possibile connettersi al server Terminal. Dopo aver verificato che si è connessi alla rete, provare a riconnettersi al server.
  - Desktop remoto disconnesso. A causa di un errore di sicurezza, il client non è stato possibile connettersi al computer remoto. Verificare che si sono connessi alla rete e quindi provare nuovamente a connettersi.

In un secondo momento, il client desktop remoto si riconnette al desktop remoto. Tuttavia, anziché il client di connettersi alla sessione originale, il server host sessione Desktop remoto si connette il client a una nuova sessione. Il controllo del server host sessione Desktop remoto rivela che la sessione originale non ha immesso uno stato disconnesso, ma invece rimane attiva.

Per risolvere questo problema, è possibile abilitare la **intervallo connessione keep-alive configura** dei criteri nel **configurazione Computer\\modelli amministrativi\\componentidiWindows\\ Servizi Desktop remoto\\Host sessione Desktop remoto\\connessioni** cartella Criteri di gruppo. Se si abilita questo criterio, è necessario immettere un intervallo keep-alive. L'intervallo keep-alive determina la frequenza, espressa in minuti, il server controlla lo stato della sessione.

Questo problema può dipendere da una configurazione errata delle impostazioni di configurazione e l'autenticazione. È possibile configurare queste impostazioni a livello di server, o mediante oggetti Criteri di gruppo. Le impostazioni dei criteri di gruppo sono disponibili nel **configurazione Computer\\modelli amministrativi\\i componenti di Windows\\Remote Desktop Services\\Host sessione Desktop remoto\\ Sicurezza** cartella Criteri di gruppo.

1. Aprire Configurazione host sessione Desktop remoto sul server Host sessione Desktop remoto.
2. Sotto **connessioni**, fare doppio clic il nome della connessione e quindi fare clic su **proprietà**.
3. Nel **delle proprietà** finestra di dialogo per la connessione, nel **generali** nella scheda **sicurezza** di livello, selezionare un metodo di sicurezza.
4. Nelle **a livello di crittografia**, fare clic sul livello desiderato. È possibile selezionare **bassa**, **compatibile con Client**, **elevata**, oppure **conforme a FIPS**.

> [!NOTE]  
>  - Quando le comunicazioni tra client e server Host sessione Desktop remoto richiedono il massimo livello di crittografia, usare la crittografia FIPS compatibile.
>  - Le impostazioni a livello di crittografia che configurano criteri di gruppo di eseguire l'override di configurazione impostate tramite lo strumento di configurazione di Servizi Desktop remoto. Inoltre, se si abilita il [crittografia di sistema: Utilizza algoritmi FIPS compatibili per crittografia, hash e firma](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)) dei criteri, questa impostazione sostituisce il **impostare il livello di crittografia connessione client** criteri. I criteri di crittografia di sistema sono nel **configurazione Computer\\delle impostazioni di Windows\\le impostazioni di sicurezza\\criteri locali\\opzioni di sicurezza** cartella.
>  - Quando si modifica il livello di crittografia, il nuovo valore impostato viene applicato al successivo accesso di un utente. Se sono necessari più livelli di crittografia in un server, installare più schede di rete e configurare ciascuna scheda separatamente.
>  - Per verificare che il certificato ha una chiave privata corrispondente, nella configurazione di Servizi Desktop remoto, fare doppio clic la connessione per il quale si desidera visualizzare il certificato, selezionare **generali**, selezionare **modificare**, selezionare il certificato che si desidera visualizzare e quindi selezionare **Visualizza certificato**. In fondo il **generale** scheda, l'istruzione "si possiede una chiave privata corrispondente al certificato" deve essere visualizzato. È anche possibile visualizzare queste informazioni utilizzando lo snap-in certificati.
>  - Crittografia FIPS compatibile (il **crittografia di sistema: Algoritmi usare FIPS compatibili per crittografia, hash e firma** dei criteri o la **FIPS compatibile** impostazione nella configurazione del Server di Desktop remoto) crittografa e decrittografa i dati inviati dal client al server e da il server al client, con gli algoritmi di crittografia 140-1 Federal informazioni Processing Standard (FIPS), usando i modelli crittografici Microsoft. Per altre informazioni, vedere [convalida di FIPS 140](https://docs.microsoft.com/en-us/windows/security/threat-protection/fips-140-validation).
>  - Il **elevata** impostazione consente di crittografare i dati inviati dal client al server e dal server al client tramite una crittografia avanzata a 128 bit.
>  - Il **compatibile con Client** impostazione consente di crittografare i dati inviati tra il client e il server la forza della chiave massima supportata dal client.
>  - Il **bassa** impostazione consente di crittografare i dati inviati dal client al server usando la crittografia a 56 bit.

## <a name="remote-laptop-disconnects-from-wireless-network"></a>Portatili remoti si disconnette dalla rete wireless

Questo problema può verificarsi quando un client desktop remoto si connette a un computer portatile tramite una rete wireless 802.1x. In modo intermittente, il computer portatile si disconnette dalla rete wireless e non viene ristabilita automaticamente.

Si tratta di un problema noto che si verifica quando l'impostazione di autenticazione di rete per la connessione di rete senza fili **l'autenticazione utente**.

Per risolvere questo problema, impostare l'impostazione di autenticazione di rete **l'autenticazione utente o computer** oppure **l'autenticazione Computer**.

 > [!NOTE]  
> Per modificare le impostazioni di autenticazione di rete in un singolo computer, si potrebbe essere necessario usare il pannello di controllo di centro rete e condivisione per creare una nuova connessione wireless con le nuove impostazioni.

Per una descrizione completa di come configurare le impostazioni di rete wireless tramite oggetti Criteri di gruppo, vedere [criteri configurare reti Wireless (IEEE 802.11)](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="user-experiences-poor-performance-or-application-problems"></a>Utente riscontra una riduzione delle prestazioni o problemi delle applicazioni

In questa sezione consente di risolvere alcuni problemi comuni che gli utenti possono riscontrare quando si usano funzionalità di desktop remoto:

  - [Gli utenti che si connettono a nuove macchine virtuali di Microsoft Azure in modo intermittente riscontrare problemi](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Gli utenti che si connettono a desktop remoto di Windows 10 versione 1709 siano presenti problemi di riproduzione video](#video-playback-issues-on-windows-10-version-1709).
  - [Gli utenti con i profili utente di sola lettura che si connettono a desktop remoto Windows 10 non è possibile condividere i propri desktop.](#desktop-sharing-issues-on-windows-10)
  - [Gli utenti la connessione a desktop remoto Windows 10 con i client che eseguono in versioni precedenti di Windows 10 riscontrare una riduzione delle prestazioni quando NLA è disabilitato](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Quando gli utenti di eseguire applicazioni multiple in sessioni di desktop remoto e disconnettersi e riconnettersi spesso, che si potrebbe verificare uno schermo nero alla riconnessione.](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problemi intermittenti con nuove macchine virtuali di Microsoft Azure

Questo problema interessa le macchine virtuali che hanno eseguito il provisioning di recente. Dopo che l'utente si connette alla macchina virtuale, la sessione desktop remoto potrebbe non essere caricato tutte le impostazioni dell'utente in modo corretto.

Per risolvere questo problema, disconnettersi dalla macchina virtuale, attendere almeno 20 minuti e quindi riconnettersi.

Per risolvere questo problema, applicare gli aggiornamenti seguenti per le macchine virtuali, come appropriato:

  - Windows 10 e Windows Server 2016: KB 4343884, [30 agosto 2018, ovvero KB4343884 (Build del sistema operativo 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891, [30 agosto 2018, ovvero KB4343891 (anteprima del Rollup mensile)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problemi relativi alla riproduzione di video su Windows 10 versione 1709

Questo problema si verifica quando gli utenti si connettono a computer remoti che eseguono Windows 10, versione 1709. Quando questi utenti riprodurre il video tramite il VMR9 codec (Video combinazione Renderer 9), Windows Media player viene solo una finestra del colore nera.

Si tratta di un problema noto in Windows 10, versione 1709. Il problema non viene eseguito in Windows 10 versione 1703.

### <a name="desktop-sharing-issues-on-windows-10"></a>Condivisione di problemi in Windows 10 desktop

Questo problema si verifica quando l'utente ha un profilo utente di sola lettura e hive del Registro di sistema associati, ad esempio in uno scenario per chiosco multimediale. Quando tale utente si connette a un computer remoto che esegue Windows 10, versione 1803, ma non possono condividere il desktop.

Per risolvere questo problema, applicare l'aggiornamento di Windows 10, 4340917 [24 luglio 2018, ovvero KB4340917 (17134.191 di Build del sistema operativo)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problemi di prestazioni quando si combinano le versioni di Windows 10 se NLA è disabilitato

Questo problema si verifica quando NLA è disabilitata, e i computer client desktop remoto che eseguono Windows 10 per connettersi a desktop remoti che eseguono versioni diverse di Windows 10. Gli utenti di client desktop remoto nei computer che eseguono Windows 10, versione 1709 o versioni precedenti riscontrare una riduzione delle prestazioni durante la connessione a desktop remoti che eseguono Windows 10, versione 1803 o versione successiva.

Ciò si verifica perché, quando NLA è disabilitata, i computer client meno recenti utilizzano un protocollo più lento quando si connettono a Windows 10, versione 1803 o versione successiva.

Per risolvere questo problema, applicare 4340917, KB [24 luglio 2018, ovvero KB4340917 (17134.191 di Build del sistema operativo)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Schermata nera

Questo problema si verifica in Windows 8.0, Windows 8.1, Windows 10 RTM e Windows Server 2012 R2. Un utente avvia più applicazioni in un desktop remoto e quindi si disconnette dalla sessione. Periodicamente, l'utente si riconnette al desktop remoto per interagire con le applicazioni e quindi scollegare nuovamente. A un certo punto, quando l'utente viene riconnessa, la sessione desktop remoto viene semplicemente mostrando uno schermo nero. L'utente deve terminare la sessione desktop remoto dalla console del computer remoto (o la console di server host sessione Desktop remoto). Questa azione arresta le applicazioni.

Per risolvere questo problema, applicare gli aggiornamenti seguenti come appropriato:

  - Windows 8 e Windows Server 2012: KB4103719, [17 maggio 2018, ovvero KB4103719 (anteprima del Rollup mensile)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 e Windows Server 2012 R2: KB4103724, [17 maggio 2018, ovvero KB4103724 (anteprima del Rollup mensile)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) KB 4284863 e [21 giugno 2018, ovvero KB4284863 (anteprima del Rollup mensile)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10: Risolto in KB4284860, [12 giugno 2018, ovvero KB4284860 (Build del sistema operativo 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
