---
title: Risoluzione dei problemi relativi alle connessioni Desktop remoto
description: Procedure di risoluzione dei problemi raggruppate in base al sintomo
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
ms.openlocfilehash: c6ce719ffa24cfc6704348c17548fe5cf33d9271
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284144"
---
# <a name="troubleshooting-remote-desktop-connections"></a>Risoluzione dei problemi relativi alle connessioni Desktop remoto
Per una breve spiegazione di alcuni dei problemi più comuni relativi a Servizi Desktop remoto, vedi [Domande frequenti sui client Desktop remoto](https://review.docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-client-faq). Questo articolo descrive vari approcci più avanzati per la risoluzione dei problemi di connessione. Molte di queste procedure consentono di eseguire la risoluzione dei problemi sia per una configurazione semplice, ad esempio un computer fisico che si connette a un altro computer fisico, sia per una configurazione più complessa. Alcune procedure invece consentono di far fronte a problemi che si verificano solo in scenari multiutente più complicati. Per altre informazioni sui componenti di Desktop remoto e su come interagiscono tra loro, vedi [Architettura di Servizi Desktop remoto](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture).

> [!NOTE]  
> Molte delle procedure descritte in questo articolo richiedono che venga eseguito l'accesso a più computer, ad alcuni dei quali potresti dover accedere in remoto. Per altre informazioni sugli strumenti di amministrazione remota e su come configurarli, vedi [Strumenti di amministrazione remota del server per sistemi operativi Windows](https://support.microsoft.com/en-us/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

Nell'elenco seguente, indentifica il tipo di sintomo che riscontri personalmente o che riscontrano i tuoi utenti.

- [Il client Desktop remoto non riesce a connettersi al desktop remoto, ma non sono presenti sintomi o messaggi specifici (procedura di risoluzione dei problemi generica)](#no-specific-symptoms-or-messages-general-troubleshooting-steps)
- [Il client Desktop remoto non riesce a connettersi al desktop remoto e riceve un messaggio "Classe non registrata"](#client-cannot-connect-class-not-registered)
- [Il client Desktop remoto non riesce a connettersi al desktop remoto e riceve un messaggio "Nessuna licenza disponibile" o "Errore di sicurezza"](#client-cannot-connect-no-licenses-available)
- [L'utente riceve un messaggio "Accesso negato" oppure deve specificare due volte le credenziali](#user-cannot-authenticate-or-must-authenticate-twice)
- [Al momento della connessione l'utente riceve un messaggio "Servizi Desktop remoto è attualmente occupato"](#on-connecting-user-receives-remote-desktop-service-is-currently-busy-message)
- [Il client Desktop remoto si disconnette e non riesce a riconnettersi alla stessa sessione](#rd-client-disconnects-and-cannot-reconnect-to-the-same-session)
- [L'utente si connette a un portatile remoto su una rete wireless e il portatile si disconnette dalla rete](#remote-laptop-disconnects-from-wireless-network)
- [L'utente riscontra prestazioni non soddisfacenti o problemi con applicazioni remote](#user-experiences-poor-performance-or-application-problems)

> [!NOTE]  
> Per un elenco aggiornato dei codici di disconnessione RDP, vedi [Enumerazione ExtendedDisconnectReasonCode](https://docs.microsoft.com/windows/desktop/TermServ/extendeddisconnectreasoncode). 

## <a name="no-specific-symptoms-or-messages-general-troubleshooting-steps"></a>Nessun sintomo o messaggio specifico (procedura di risoluzione dei problemi generica)

Segui questa procedura quando un client Desktop remoto non riesce a connettersi a un desktop remoto, ma non fornisce messaggi o altri sintomi utili per identificare la causa. Per risolvere molte delle cause più comuni di questo tipo di problema, sono disponibili i metodi seguenti:

- [Verificare lo stato del protocollo RDP](#check-the-status-of-the-rdp-protocol)
- [Verificare lo stato dei servizi RDP](#check-the-status-of-the-rdp-services)
- [Verificare che il listener RDP sia funzionante](#check-that-the-rdp-listener-is-functioning)
- [Verificare la porta del listener RDP](#check-the-rdp-listener-port)

### <a name="check-the-status-of-the-rdp-protocol"></a>Verificare lo stato del protocollo RDP

#### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Verificare lo stato del protocollo RDP in un computer locale

Per verificare e modificare lo stato del protocollo RDP in un computer locale, vedi [Come abilitare Desktop remoto](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Se le opzioni Desktop remoto non sono disponibili, vedi [Verificare se un oggetto Criteri di gruppo blocca RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

#### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Verificare lo stato del protocollo RDP in un computer remoto

> [!IMPORTANT]  
> Segui con attenzione la procedura descritta in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verifichino problemi.

Per verificare e modificare lo stato del protocollo RDP in un computer remoto, usa una connessione al Registro di sistema in rete:

1. Fai clic su **Start**, scegli **Esegui** e quindi immetti **regedt32**.
2. Nell'editor del Registro di sistema scegli **Connetti a Registro di sistema in rete** dal menu **File**.
3. Nella finestra di dialogo **Seleziona computer** immetti il nome del computer remoto, fai clic su **Controlla nomi** e quindi su **OK**.
4. Passa a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**.  
   ![Editor del Registro di sistema, in cui viene mostrata la voce fDenyTSConnections](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - Se il valore della chiave **fDenyTSConnections** è **0**, RDP è abilitato
   - Se il valore della chiave **fDenyTSConnections** è **1**, RDP è disabilitato
5. Per abilitare RDP, modifica il valore di **fDenyTSConnections** sostituendo **1** con **0**.

#### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Verificare se un oggetto Criteri di gruppo blocca RDP in un computer locale

Se non riesci ad attivare RDP nell'interfaccia utente o se il valore di **fDenyTSConnections** si reimposta su **1** dopo che è stato modificato, è possibile che un oggetto Criteri di gruppo esegua l'override delle impostazioni a livello di computer.

Per verificare la configurazione di Criteri di gruppo in un computer locale, apri una finestra del prompt dei comandi come amministratore e immetti il comando seguente:
```
gpresult /H c:\gpresult.html
```
Al termine del comando, apri gpresult.html. In **Configurazione computer\\Modelli amministrativi\\Componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\Connessioni** trova il criterio **Consenti la connessione remota tramite Servizi Desktop remoto**.

- Se l'impostazione di questo criterio è **Abilitato**, i criteri di gruppo non bloccano le connessioni RDP.
- Se l'impostazione di questo criterio è **Disabilitato**, controlla **Oggetto Criteri di gruppo dominante**. Questo è l'oggetto Criteri di gruppo che blocca le connessioni RDP.
  ![Segmento di esempio di gpresult.html, in cui l'oggetto Criteri di gruppo a livello di dominio **Block RDP** disabilita RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Segmento di esempio di gpresult.html, in cui **Local Group Policy** disabilita RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

#### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Verificare se un oggetto Criteri di gruppo blocca RDP in un computer remoto

Per verificare la configurazione di Criteri di gruppo in un computer remoto, il comando è quasi identico a quello per un computer locale:
```
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```
Il file prodotto da questo comando (**gpresult-\<nome computer\>.html**) usa per le informazioni lo stesso formato usato dalla versione per il computer locale (**gpresult.html**).

#### <a name="modifying-a-blocking-gpo"></a>Modifica di un oggetto Criteri di gruppo che blocca

Puoi modificare queste impostazioni nell'Editor oggetti Criteri di gruppo e nella Console Gestione Criteri di gruppo. Per altre informazioni su come usare i criteri di gruppo, vedi [Gestione avanzata di Criteri di gruppo](https://docs.microsoft.com/microsoft-desktop-optimization-pack/agpm/).

Per modificare il criterio che blocca, usa uno dei metodi seguenti:

- Nell'Editor oggetti Criteri di gruppo accedi al livello appropriato di oggetti Criteri di gruppo (ad esempio, locali o di dominio) e passa a **Configurazione computer\\Modelli amministrativi\\Componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\Connessioni**\\**Consenti la connessione remota tramite Servizi Desktop remoto**.  
   1. Imposta il criterio su **Abilitato** o **Non configurato**.
   2. Nei computer interessati apri una finestra del prompt dei comandi come amministratore ed esegui il comando **gpupdate /force**.
- Nella Console Gestione Criteri di gruppo passa all'unità organizzativa in cui il criterio che blocca è applicato ai computer interessati ed elimina tale criterio dall'unità organizzativa.

### <a name="check-the-status-of-the-rdp-services"></a>Verificare lo stato dei servizi RDP

Sia nel computer (client) locale sia nel computer (di destinazione) remoto devono essere in esecuzione i servizi seguenti:

- Servizi Desktop remoto (TermService)
- Redirector porta UserMode di Servizi Desktop remoto (UmRdpService)

Puoi usare lo snap-in Servizi di MMC per gestire i servizi in locale o in remoto. Puoi anche usare PowerShell in locale o in remoto se il computer remoto è configurato in modo da accettare comandi di PowerShell remoti.

![Servizi di Desktop remoto nello snap-in Servizi di MMC. Non modificare le impostazioni predefinite dei servizi.](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

In tutti e due i computer, se uno o entrambi i servizi non sono in esecuzione, avviali.

> [!NOTE]  
> Se avvii Servizi Desktop remoto, fai clic su **Sì** per riavviare automaticamente il servizio Redirector porta UserMode di Servizi Desktop remoto.

### <a name="check-that-the-rdp-listener-is-functioning"></a>Verificare che il listener RDP sia funzionante

> [!IMPORTANT]  
> Segui con attenzione la procedura descritta in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verifichino problemi.

#### <a name="check-the-status-of-the-rdp-listener"></a>Verificare lo stato del listener RDP

Per questa procedura, usa un'istanza di PowerShell con autorizzazioni amministrative. Per un computer locale, puoi anche usare un prompt dei comandi con autorizzazioni amministrative. In questa procedura viene tuttavia usato PowerShell perché gli stessi comandi funzionano sia in locale che in remoto.

1. Aprire una finestra di PowerShell. Per eseguire la connessione a un computer remoto, immetti **Enter-PSSession -ComputerName \<nome computer\>** .
2. Immetti **qwinsta**. 
    ![Il comando qwinsta elenca i processi in ascolto sulle porte del computer.](../media/troubleshoot-remote-desktop-connections/WPS_qwinsta.png)
3. Se l'elenco include **rdp-tcp** con stato **Listen**, il listener RDP funziona. Passa a [Verificare la porta del listener RDP](#check-the-rdp-listener-port). In caso contrario, continua con il passaggio 4.
4. Esporta la configurazione del listener RDP da un computer funzionante.
    1. Accedi a un computer che abbia un sistema operativo della stessa versione del computer interessato e passa al Registro di sistema di tale computer, ad esempio tramite l'editor del Registro di sistema.
    2. Passa alla voce seguente del Registro di sistema:  
        **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\RDP-Tcp**
    3. Esporta la voce in un file con estensione reg. Ad esempio, nell'editor del Registro di sistema fai clic con il pulsante destro del mouse sulla voce, scegli **Esporta** e quindi immetti un nome file per le impostazioni esportate.
    4. Copia nel computer interessato il file con estensione reg esportato.
5. Per importare la configurazione del listener RDP, apri una finestra di PowerShell con autorizzazioni amministrative nel computer interessato oppure apri la finestra di PowerShell ed esegui in remoto la connessione al computer interessato.
   1. Per eseguire il backup della voce esistente del Registro di sistema, immetti il comando seguente:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Per rimuovere la voce esistente del Registro di sistema, immetti i comandi seguenti:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Per importare la nuova voce del Registro di sistema e quindi riavviare il servizio, immetti i comandi seguenti:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```   
      dove \<filename\> è il nome del file con estensione reg esportato.
6. Testa la connessione provando a eseguire di nuovo la connessione al desktop remoto. Se ancora non riesci a connetterti, riavvia il computer interessato.
7. Se il problema persiste, [verifica lo stato del certificato autofirmato RDP](#check-the-status-of-the-rdp-self-signed-certificate).

#### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Verificare lo stato del certificato autofirmato RDP

1. Se ti è ancora impossibile eseguire la connessione, apri lo snap-in Certificati di MMC. Quando ti viene chiesto di selezionare l'archivio certificati da gestire, seleziona **Account del computer** e quindi il computer interessato.
2. Nella sottocartella **Certificati** di **Desktop remoto** elimina il certificato autofirmato RDP. 
    ![Certificati di Desktop remoto nello snap-in Certificati di MMC.](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. Nel computer interessato riavvia Servizi Desktop remoto.
4. Aggiorna lo snap-in Certificati.
5. Se il certificato autofirmato RDP non è stato ricreato, [verifica le autorizzazioni della cartella MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

#### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Verificare le autorizzazioni della cartella MachineKeys

1. Nel computer interessato apri Esplora risorse e quindi passa a **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Fai clic con il pulsante destro del mouse su **MachineKeys**, scegli **Proprietà**, fai clic su **Sicurezza** e quindi su **Avanzate**.
3. Assicurati che siano configurate le autorizzazioni seguenti:
      - Builtin\\Administrators: Controllo completo
      - Everyone: Lettura, Scrittura

### <a name="check-the-rdp-listener-port"></a>Verificare la porta del listener RDP

Sia nel computer (client) locale sia nel computer (di destinazione) remoto il listener RDP deve essere in ascolto sulla porta 3389. Nessun'altra applicazione deve usare questa porta.

> [!IMPORTANT]  
> Segui con attenzione la procedura descritta in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verifichino problemi.

Per verificare o modificare la porta RDP, usa l'editor del Registro di sistema:

1. Fai clic su Start, scegli **Esegui** e quindi immetti **regedt32**.
      - Per eseguire la connessione a un computer remoto, scegli **Connetti a Registro di sistema in rete** dal menu **File**.
      - Nella finestra di dialogo **Seleziona computer** immetti il nome del computer remoto, fai clic su **Controlla nomi** e quindi su **OK**.
2. Apri il Registro di sistema e passa a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>** . 
    ![Sottochiave PortNumber per il protocollo RDP.](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. Se **PortNumber** ha un valore diverso da **3389**, impostalo su **3389**. 
   > [!IMPORTANT]  
    > Puoi far funzionare i servizi di Desktop remoto usando un'altra porta. Non è tuttavia consigliabile farlo. La risoluzione dei problemi per una tale configurazione non rientra negli obiettivi di questo articolo.
4. Dopo aver modificato il numero di porta, riavvia Servizi Desktop remoto.

#### <a name="check-that-another-application-is-not-trying-to-use-the-same-port"></a>Verificare che un'altra applicazione non stia tentando di usare la stessa porta

Per questa procedura, usa un'istanza di PowerShell con autorizzazioni amministrative. Per un computer locale, puoi anche usare un prompt dei comandi con autorizzazioni amministrative. In questa procedura viene tuttavia usato PowerShell perché gli stessi comandi funzionano in locale e in remoto.

1. Aprire una finestra di PowerShell. Per eseguire la connessione a un computer remoto, immetti **Enter-PSSession -ComputerName \<nome computer\>** .
2. Immettere il comando seguente:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![Il comando netstat genera un elenco di porte e dei servizi in ascolto su di esse.](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. Cercare una voce per la porta TCP 3389 (o la porta assegnata a RDP) con lo stato **In ascolto**. 
    > [!NOTE]  
   > Il PID del processo o del servizio che utilizza la porta è visualizzato nella colonna PID.
4. Per determinare quale applicazione usa la porta 3389 (o la porta RDP assegnata), immetti il comando seguente:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![Il comando tasklist fornisce i dettagli di un processo specifico.](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. Cerca una voce per il numero PID associato alla porta (fornito dall'output di **netstat**). I servizi o i processi associati a tale PID vengono visualizzati sulla destra.
6. Se la porta viene usata da un'applicazione o da un servizio diverso da Servizi Desktop remoto (TermServ.exe), puoi risolvere il conflitto usando uno dei metodi seguenti:
      - Configura l'altra applicazione o l'altro servizio in modo che usi una porta diversa (scelta consigliata).
      - Disinstalla l'altra applicazione o l'altro servizio.
      - Configura RDP in modo che usi una porta diversa e quindi riavvia Servizi Desktop remoto (scelta non consigliata).

#### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Verificare se la porta RDP è bloccata da un firewall

Usa lo strumento **psping** per verificare se puoi raggiungere il computer interessato tramite la porta 3389.

1. In un computer diverso da quello interessato scarica **psping** da <https://live.sysinternals.com/psping.exe>.
2. Apri una finestra del prompt dei comandi come amministratore, passa alla directory in cui hai installato **psping** e quindi immetti il comando seguente:  
   
   ```  
   psping -accepteula <computer IP>:3389  
   ```
   
3. Verifica se nell'output del comando **psping** sono presenti risultati simili ai seguenti:  
      - **Connessione a \<IP computer\>** : il computer remoto è raggiungibile.
      - **(0% loss)** : tutti i tentativi di connessione hanno avuto esito positivo.
      - **Il computer remoto ha rifiutato la connessione di rete**: il computer remoto non è raggiungibile.
      - **(100% loss)** : tutti i tentativi di connessione hanno avuto esito negativo.
1. Esegui **psping** in più computer per verificarne la capacità di connettersi al computer interessato.
1. Nota se il computer interessato blocca le connessioni da tutti i computer, da qualche altro computer o solo da un altro computer.
1. Passaggi successivi consigliati:
      - Chiedi agli amministratori di rete di verificare che la rete consenta il traffico RDP verso il computer interessato.
      - Esamina le configurazioni degli eventuali firewall tra i computer di origine e il computer interessato (incluso Windows Firewall nel computer interessato) per determinare se la porta RDP viene bloccata da un firewall.

## <a name="client-cannot-connect-class-not-registered"></a>Il client non riesce a connettersi, "Classe non registrata"

Quando provi a connetterti a un computer remoto usando un client che esegue Windows 10 versione 1709 o successive, può accadere che il client non riesca a connettersi mentre il server Host sessione Desktop remoto invia un messaggio contenente il codice di errore "Classe non registrata (0x80040154)".

Questo problema si verifica se l'utente che tenta di connettersi ha un profilo utente obbligatorio. Per risolverlo, installa l'aggiornamento di Windows 10 del [24 luglio 2018 - KB4338817 (build sistema operativo 16299.579)](https://support.microsoft.com/en-us/help/4338817/windows-10-update-kb4338817).

## <a name="client-cannot-connect-no-licenses-available"></a>Il client non riesce a connettersi, nessuna licenza disponibile

Questa situazione si verifica per le distribuzioni che includono un server Host sessione Desktop remoto e un server licenze Desktop remoto.

Identifica il tipo di comportamento rilevato dagli utenti:

- La sessione è stata disconnessa perché non sono disponibili licenze o server licenze
- L'accesso è stato negato a causa di un errore di sicurezza

Accedi all'host sessione Desktop remoto come amministratore di dominio e apri lo strumento Diagnosi Servizio licenze Desktop remoto. Cerca messaggi simili ai seguenti:

  - Il periodo di prova per il server Host sessione Desktop remoto è scaduto, ma tale server non è stato configurato con alcun server licenze. Le connessioni al server Host sessione Desktop remoto verranno negate a meno che non sia configurato un server licenze per tale server.
  - Server licenze \<nome computer\> non disponibile. Ciò può verificarsi a causa di problemi relativi alla connettività di rete, all'arresto del Servizio licenze Desktop remoto nel server licenze oppure all'assenza del Servizio licenze Desktop remoto nel computer.

Questi problemi tendono a essere associati ai messaggi utente seguenti:

  - La sessione remota è stata disconnessa perché non sono disponibili licenze CAL (Client Access License) di Desktop remoto per il PC.
  - La sessione remota è stata disconnessa perché non sono disponibili server licenze di Desktop remoto per il rilascio della licenza.

In questo caso, [configura il servizio licenze Desktop remoto](#configure-the-rd-licensing-service).

Se lo strumento Diagnosi Servizio licenze Desktop remoto segnala altri problemi, ad esempio "Il componente del protocollo RDP X.224 ha rilevato un errore nel flusso del protocollo e ha disconnesso il client", può essere presente un problema che incide sui certificati di licenza. Tali problemi tendono a essere associati a messaggi utente simili al seguente:

Impossibile connettersi al server terminal. Errore di sicurezza. Dopo aver verificato di essere connessi alla rete, provare a riconnettersi al server.

In questo caso, [aggiorna le chiavi del Registro di sistema per il certificato X509](#refresh-the-x509-certificate-registry-keys).

### <a name="configure-the-rd-licensing-service"></a>Configurare il servizio licenze Desktop remoto

Nella procedura seguente viene usato Server Manager per apportare le modifiche relative alla configurazione. Per informazioni su come configurare e usare Server Manager, vedi [Server Manager](https://docs.microsoft.com/windows-server/administration/server-manager/server-manager).

1. Apri Server Manager e quindi passa a **Servizi Desktop remoto**.
2. In **Panoramica della distribuzione** seleziona **Attività** e quindi **Modifica proprietà distribuzione**.
3. Seleziona **Servizio licenze Desktop remoto** e quindi la modalità gestione licenze appropriata per la distribuzione (**Per Dispositivo** o **Per Utente**).
4. Immetti il nome di dominio completo del server licenze Desktop remoto e quindi fai clic su **Aggiungi**.
5. Se disponi di più server licenze Desktop remoto, ripeti il passaggio 4 per ogni server. 
    ![Opzioni di configurazione del server licenze Desktop remoto in Server Manager.](../media/troubleshoot-remote-desktop-connections/RDLicensing_Configure.png)

### <a name="refresh-the-x509-certificate-registry-keys"></a>Aggiornare le chiavi del Registro di sistema per il certificato X509

> [!IMPORTANT]  
> Segui con attenzione la procedura descritta in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verifichino problemi.

Per risolvere questo problema, esegui il backup e quindi rimuovi le chiavi del Registro di sistema relative al certificato X509, riavvia il computer e infine riattiva il server licenze Desktop remoto. Per farlo, eseguire le seguenti operazioni:

> [!NOTE]  
> Esegui la procedura seguente in ognuno dei server Host sessione Desktop remoto.

1. Apri l'editor del Registro di sistema e passa a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\RCM**.
2. Scegli **Esporta file del Registro di sistema** dal menu del Registro di sistema.
3. Digita **exported- Certificate** nella casella **Nome file** e quindi fai clic su **Salva**.
4. Fai clic con il pulsante destro del mouse su ognuno dei valori seguenti, scegli **Elimina** e quindi fai clic su **Sì** per confermare l'eliminazione:  
      - **Certificato**
      - **Certificato X509**
      - **ID Certificato X509**
      - **X509 Certificate2**
5. Esci dall'editor del Registro di sistema e quindi riavvia il server Host sessione Desktop remoto.

## <a name="user-cannot-authenticate-or-must-authenticate-twice"></a>L'utente non riesce ad autenticarsi o deve autenticarsi due volte

Sono diversi i fattori che possono causare problemi che incidono sull'autenticazione dell'utente. Questa sezione tratta i casi seguenti:

  - [A un utente viene negato l'accesso con un messaggio che indica un tipo di accesso con restrizioni](#access-denied-restricted-type-of-logon)
  - [A un utente o a un'applicazione viene negato l'accesso con l'evento 16965, "Una chiamata remota al database SAM è stata negata"](#access-denied-a-remote-call-to-the-sam-database-has-been-denied)
  - [Un utente non riesce ad accedere usando una smart card](#a-user-cannot-sign-in-by-using-a-smart-card)
  - [Se il PC remoto è bloccato, un utente deve immettere due volte la password](#if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice)
  - [Un utente non riesce ad accedere e riceve messaggi che indicano un "errore di autenticazione" e la "correzione oracolo di crittografia CredSSP"](#user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages)
  - [Dopo l'aggiornamento dei computer client, alcuni utenti devono accedere due volte](#after-you-update-client-computers-some-users-need-to-sign-in-twice)
  - [Agli utenti viene negato l'accesso per una distribuzione che usa RemoteGuard con più gestori connessione Desktop remoto](#users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers)

### <a name="access-denied-restricted-type-of-logon"></a>Accesso negato, tipo di accesso con restrizioni

In questa situazione a un utente Windows 10 che tenta di connettersi a computer Windows 10 o Windows Server 2016 viene negato l'accesso con un messaggio simile al seguente:

> Connessione Desktop remoto:  
> L'amministratore di sistema ha limitato il tipo di accesso (in rete o interattivo) che è possibile usare. Per assistenza, contattare l'amministratore di sistema o il supporto tecnico.

Questo problema si verifica quando è necessaria l'Autenticazione a livello di rete per le connessioni RDP e l'utente non è membro del gruppo **Utenti desktop remoto**. Può verificarsi anche se il gruppo **Utenti desktop remoto** non è stato assegnato al diritto utente **Accedi al computer dalla rete**.

Per risolvere questo problema, esegui uno di questi passaggi:

  - [Modifica l'appartenenza dell'utente al gruppo o l'assegnazione dei diritti utente](#modify-the-users-group-membership-or-user-rights-assignment).
  - Disattiva l'Autenticazione a livello di rete (scelta non consigliata).
  - Usa client Desktop remoto diversi da Windows 10. Ad esempio, i client Windows 7 non presentano questo problema.

#### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modificare l'appartenenza dell'utente al gruppo o l'assegnazione dei diritti utente

Se questo problema riguarda un utente singolo, la soluzione più semplice è quella di aggiungere tale utente al gruppo **Utenti desktop remoto**.

Se l'utente fa già parte di questo gruppo (o se più membri del gruppo hanno lo stesso problema), verifica la configurazione dei diritti utente nel computer Windows 10 o Windows Server 2016 remoto.

1. Apri l'Editor oggetti Criteri di gruppo ed effettua la connessione al criterio locale del computer remoto.
2. Passa a **Configurazione computer\\Impostazioni di Windows\\Impostazioni sicurezza\\Criteri locali\\Assegnazione diritti utente**, fai clic con il pulsante destro del mouse su **Accedi al computer dalla rete** e quindi scegli **Proprietà**.
3. Verifica l'elenco degli utenti e dei gruppi per **Utenti desktop remoto** (o un gruppo padre).
4. Se l'elenco non include **Utenti desktop remoto** (o un gruppo padre come ad esempio **Everyone**), devi aggiungerlo all'elenco (se nella distribuzione sono presenti più di uno o due computer, usa un oggetto Criteri di gruppo).  
    Ad esempio, come membro predefinito per **Accedi al computer dalla rete** è incluso **Everyone**. Se la distribuzione usa un oggetto Criteri di gruppo per rimuovere **Everyone**, potresti dover ripristinare l'accesso aggiornando tale oggetto in modo da aggiungere **Utenti desktop remoto**.

### <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Accesso negato, una chiamata remota al database SAM è stata negata

Questo comportamento si verifica molto probabilmente se i controller di dominio eseguono Windows Server 2016 o versioni successive e gli utenti tentano di connettersi usando un'app di connessione personalizzata. In particolare, l'accesso verrà negato alle applicazioni che accedono alle informazioni del profilo dell'utente in Active Directory.

Tale comportamento dipende da una modifica apportata a Windows. In Windows Server 2012 R2 e versioni precedenti, quando un utente accede a un desktop remoto, Connection Manager di Accesso remoto contatta il controller di dominio per richiedere le configurazioni specifiche di Desktop remoto mediante query sull'oggetto utente in Active Directory Domain Services (AD DS). Queste informazioni vengono visualizzate nella scheda Profilo di Servizi Desktop remoto delle proprietà dell'oggetto di un utente nello snap-in di MMC Utenti e computer di Active Directory.

A partire da Windows Server 2016, Connection Manager di Accesso remoto non esegue più query sull'oggetto utente in AD DS. Se hai bisogno di Connection Manager di Accesso remoto per eseguire query su AD DS perché usi gli attributi di Servizi Desktop remoto, devi abilitare manualmente il comportamento precedente di Connection Manager di Accesso remoto.

> [!IMPORTANT]  
> Segui con attenzione la procedura descritta in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756%22%20target=%22_self%22) nel caso in cui si verifichino problemi.

Per abilitare il comportamento legacy di Connection Manager di Accesso remoto in un server Host sessione Desktop remoto, configura le voci seguenti del Registro di sistema e quindi riavvia il servizio **Servizi Desktop remoto**:  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<nome WinStation\>\\**  
      - Nome: **fQueryUserConfigFromDC**
      - Digitare il comando seguente: **Reg\_DWORD**
      - Valore: **1** (Decimal)

Per abilitare il comportamento legacy di Connection Manager di Accesso remoto in un server che non sia un server Host sessione Desktop remoto, configura queste voci del Registro di sistema e la voce aggiuntiva seguente (e quindi riavvia il servizio):
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

Per altre informazioni su questo comportamento, vedi l'articolo della Knowledge Base 3200967, [Modifiche relative a Connection Manager di Accesso remoto in Windows Server](https://support.microsoft.com/en-us/help/3200967/changes-to-remote-connection-manager-in-windows-server).

### <a name="a-user-cannot-sign-in-by-using-a-smart-card"></a>Un utente non riesce ad accedere usando una smart card

Questo articolo illustra tre circostanze comuni in cui un utente non riesce ad accedere a un desktop remoto mediante una smart card:  
  - [L'utente non riesce ad accedere a una succursale che usa un controller di dominio di sola lettura](#cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc)
  - [L'utente non riesce ad accedere a un computer Windows Server 2008 SP2 aggiornato di recente](#cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer)
  - [L'utente non riesce a rimanere connesso a un computer Windows aggiornato di recente e Servizi Desktop remoto smette di rispondere (si blocca)](#cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs)

#### <a name="cannot-sign-in-with-a-smart-card-in-a-branch-office-with-a-rodc"></a>Impossibilità di accedere mediante una smart card in una succursale con un controller di dominio di sola lettura

Questo problema si verifica nelle distribuzioni che includono un server Host sessione Desktop remoto presso una succursale che usa un controller di dominio di sola lettura. Il server Host sessione Desktop remoto è ospitato nel dominio radice. Gli utenti nella succursale appartengono a un dominio figlio e usano smart card per l'autenticazione. Il controller di dominio di sola lettura è configurato in modo da memorizzare nella cache le password utente (tale controller appartiene al gruppo **Ogg. autorizzati a replica passw. in controller sola lettura**). Quando gli utenti tentano di accedere a sessioni nel server Host sessione Desktop remoto, ricevono messaggi del tipo "Il tentativo di accesso non è valido a causa di un nome utente o di informazioni di autenticazione non validi".

Si tratta di un problema noto riguardante il modo in cui il controller di dominio radice e il controller di dominio di sola lettura gestiscono la crittografia delle credenziali utente. Il primo usa una chiave di crittografia per crittografare le credenziali, mentre il secondo fornisce una chiave di decrittografia al client. Le due chiavi però non corrispondono.

Per ovviare a questo problema, considera la possibilità di cambiare topologia di controller di dominio disattivando la memorizzazione delle password nella cache sul controller di dominio di sola lettura oppure distribuendo alla succursale un controller di dominio scrivibile. Un metodo alternativo consiste nello spostare il server Host sessione Desktop remoto nello stesso dominio figlio in cui si trovano gli utenti. Un'altra alternativa è quella di consentire agli utenti di accedere senza smart card. Ognuna di queste soluzioni può rendere necessario un compromesso tra le prestazioni e il livello di sicurezza.

#### <a name="cannot-sign-in-with-a-smart-card-to-a-windows-server-2008-sp2-computer"></a>Impossibilità di accedere mediante una smart card a un computer Windows Server 2008 SP2

Questo problema si verifica quando gli utenti accedono a un computer Windows Server 2008 SP2 aggiornato con KB4093227 (2018.4B). Se gli utenti tentano di accedere usando una smart card, viene loro negato l'accesso con messaggi del tipo "Nessun certificato valido trovato. Controllare che la scheda sia inserita correttamente". Allo stesso tempo, il computer Windows Server registra l'evento applicazione "Errore durante la ricerca di un certificato digitale dalla smart card inserita: Firma non valida".

Per risolvere questo problema, aggiorna il computer Windows Server con il nuovo rilascio 2018.06 B oggetto dell'articolo della Knowledge Base 4093227, [Descrizione dell'aggiornamento di sicurezza per la vulnerabilità di tipo Denial of Service di Windows Remote Desktop Protocol (RDP) in Windows Server 2008: 10 aprile 2018](https://support.microsoft.com/en-us/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

#### <a name="cannot-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>Impossibilità di restare connessi mediante una smart card e blocco di Servizi Desktop remoto

Questo problema si verifica quando gli utenti accedono a un computer Windows o Windows Server aggiornato con KB 4056446. L'utente inizialmente può riuscire ad accedere al sistema mediante una smart card, ma successivamente riceve il messaggio di errore "SCARD\_E\_NO\_SERVICE". Il computer remoto potrebbe smettere di rispondere.

Per ovviare a questo problema, riavvia il computer remoto.

Per risolvere questo problema, aggiorna il computer remoto con la correzione appropriata:

  - Windows Server 2008 SP2: KB 4090928, [Windows perde gli handle nel processo lsm.exe e le applicazioni smart card possono visualizzare errori "SCARD\_E\_NO\_SERVICE"](https://support.microsoft.com/en-us/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB 4103724, [17 maggio 2018 - KB4103724 (anteprima del rollup mensile)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 e Windows 10, versione 1607: KB 4103720, [17 maggio 2018 - KB4103720 (build sistema operativo 14393.2273)](https://support.microsoft.com/en-us/help/4103720/windows-10-update-kb4103720)

### <a name="if-the-remote-pc-is-locked-a-user-needs-to-enter-a-password-twice"></a>Se il PC remoto è bloccato, un utente deve immettere due volte la password

Questo problema può verificarsi quando un utente tenta di connettersi a un desktop remoto che esegue Windows 10 versione 1709 in una distribuzione in cui le connessioni RDP non richiedono l'Autenticazione a livello di rete. In queste condizioni, se il desktop remoto è stato bloccato, l'utente deve immettere due volte le credenziali quando si connette.

Per risolvere questo problema, aggiorna il computer Windows 10 versione 1709 con KB 4343893, [30 agosto 2018 - KB4343893 (build sistema operativo 16299.637)](https://support.microsoft.com/en-us/help/4343893/windows-10-update-kb4343893).

### <a name="user-cannot-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>Un utente non riesce ad accedere e riceve messaggi che indicano un "errore di autenticazione" e la "correzione oracolo di crittografia CredSSP"

Quando gli utenti tentano di accedere usando una versione qualsiasi di Windows, da Windows Vista SP2 e versioni successive a Windows Server 2008 SP2 e versioni successive, viene loro negato l'accesso e vengono visualizzati messaggi simili ai seguenti:

  - Si è verificato un errore di autenticazione. La funzione richiesta non è supportata.
  - Ciò potrebbe dipendere dalla correzione oracolo di crittografia CredSSP

"Correzione oracolo di crittografia CredSSP" indica un set di aggiornamenti di sicurezza rilasciati a marzo, aprile e maggio 2018. CredSSP è un provider di autenticazione che elabora le richieste di autenticazione per altre applicazioni. L'aggiornamento "3B" del 13 marzo 2018 e gli aggiornamenti successivi hanno riguardato un exploit in cui l'autore di un attacco poteva inoltrare le credenziali utente per eseguire codice sul sistema di destinazione.

Gli aggiornamenti iniziali hanno aggiunto il supporto per un nuovo oggetto Criteri di gruppo, **Correzione oracolo di crittografia**, che può avere le impostazioni seguenti:

  - **Vulnerabile**. Le applicazioni client che usano CredSSP possono eseguire il fallback a versioni non sicure, ma questo comportamento espone i desktop remoti ad attacchi. I servizi che usano CredSSP accettano i client che non sono stati aggiornati.
  - **Mitigato**. Le applicazioni client che usano CredSSP non possono eseguire il fallback a versioni non sicure, ma i servizi che usano CredSSP accettano i client che non sono stati aggiornati.
  - **Forza client aggiornati**. Le applicazioni client che usano CredSSP non possono eseguire il fallback a versioni non sicure e i servizi che usano CredSSP non accetteranno i client senza patch. 
    Nota: questa impostazione deve essere distribuita solo dopo che tutti gli host remoti supportano la versione più recente.

L'aggiornamento dell'8 maggio 2018 ha modificato l'impostazione predefinita di **Correzione oracolo di crittografia** da **Vulnerabile** a **Mitigato**. A causa di tale modifica, i client Desktop remoto con gli aggiornamenti non possono connettersi a server che non li hanno (o a server aggiornati che non sono stati riavviati). Per altre informazioni sugli effetti degli aggiornamenti e sui tipi di comunicazione che bloccano, vedi l'articolo della Knowledge Base 4093492, [Aggiornamenti CredSSP per CVE-2018-0886](https://support.microsoft.com/en-us/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Per risolvere questo problema, assicurati che tutti i sistemi siano stati completamente aggiornati e riavviati. Per un elenco completo degli aggiornamenti e altre informazioni sulle vulnerabilità, vedi [CVE-2018-0886 | Vulnerabilità relativa all'esecuzione di codice in remoto CredSSP](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/CVE-2018-0886).

Per ovviare a questo problema fino al completamento degli aggiornamenti, leggi l'articolo della Knowledge Base 4093492 per avere informazioni sui tipi di connessioni consentiti. Se non sono disponibili alternative fattibili, considera la possibilità di procedere in uno dei modi seguenti:

- Per i computer client interessati, reimposta il criterio **Correzione oracolo di crittografia** su **Vulnerabile**.
- Modifica i criteri seguenti nella cartella di Criteri di gruppo **Configurazione computer\\Modelli amministrativi\\Componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\Sicurezza**:  
  - **Richiedi l'utilizzo di un livello di sicurezza specificato per le connessioni remote (RDP)** : imposta su **Abilitato** e seleziona **RDP**.
  - **Richiedi autenticazione utente tramite Autenticazione a livello di rete per le connessioni remote**: imposta su **Disabilitato**.
    > [!IMPORTANT]  
    > Queste modifiche rendono meno sicura la distribuzione. Se decidi di apportarle, devono essere solo temporanee.

Per altre informazioni sulla gestione dei criteri di gruppo, vedi [Modifica di un oggetto Criteri di gruppo che blocca](#modifying-a-blocking-gpo).

### <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Dopo l'aggiornamento dei computer client, alcuni utenti devono accedere due volte

Gli utenti in alcuni computer Windows 7 o Windows 10 versione 1709 accedono a una sessione Desktop remoto e vedono immediatamente una seconda schermata di accesso. Questo problema si verifica se i computer client hanno ricevuto gli aggiornamenti seguenti:

  - Windows 7: KB 4103718, [8 maggio 2018 - KB4103718 (rollup mensile)](https://support.microsoft.com/en-us/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB 4103727, [8 maggio 2018 - KB4103727 (build sistema operativo 16299.431)](https://support.microsoft.com/en-us/help/4103727/windows-10-update-kb4103727)

Per risolvere questo problema, assicurati che i computer a cui gli utenti vogliono connettersi (e i server Host sessione Desktop remoto o RDVI) siano stati completamente aggiornati fino a giugno 2018. Sono inclusi gli aggiornamenti seguenti:

  - Windows Server 2016: KB 4284880, [12 giugno 2018 - KB4284880 (build sistema operativo 14393.2312)](https://support.microsoft.com/en-us/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815, [12 giugno 2018 - KB4284815 (rollup mensile)](https://support.microsoft.com/en-us/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855, [12 giugno 2018 - KB4284855 (rollup mensile)](https://support.microsoft.com/en-us/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826, [12 giugno 2018 - KB4284826 (rollup mensile)](https://support.microsoft.com/en-us/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB 4056564, [Descrizione dell'aggiornamento di sicurezza per la vulnerabilità relativa all'esecuzione di codice in remoto CredSSP in Windows Server 2008, Windows Embedded POSReady 2009 e Windows Embedded Standard 2009: 13 marzo 2018](https://support.microsoft.com/en-us/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

### <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>Agli utenti viene negato l'accesso per una distribuzione che usa Remote Credential Guard con più gestori connessione Desktop remoto

Questo problema si verifica nelle distribuzioni a disponibilità elevata che usano due o più gestori connessione Desktop remoto, se è in uso Windows Defender Remote Credential Guard. Gli utenti non riescono ad accedere ai desktop remoti.

Questo problema si verifica perché Remote Credential Guard usa Kerberos per l'autenticazione e limita NTLM. In una configurazione a disponibilità elevata con bilanciamento del carico i gestori connessione Desktop remoto, tuttavia, non possono supportare le operazioni Kerberos.

Se hai necessità di usare una tale configurazione con gestori connessione Desktop remoto con bilanciamento del carico, puoi ovviare a questo problema disabilitando Remote Credential Guard. Per altre informazioni su come gestire Windows Defender Remote Credential Guard, vedi [Proteggere le credenziali di Desktop remoto con Windows Defender Remote Credential Guard](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).

## <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Al momento della connessione l'utente riceve un messaggio "Servizi Desktop remoto è attualmente occupato"

Per determinare la risposta appropriata per questo problema, segui questa procedura:

1. Servizi Desktop remoto smette di rispondere (ad esempio, il client Desktop remoto sembra "bloccarsi" sulla schermata iniziale)?  
      - Se il servizio non risponde più, vedi [Problema di memoria del server Host sessione Desktop remoto](#rdsh-server-memory-issue).
      - Se il client sembra interagire normalmente con il servizio, prosegui con il passaggio successivo.
2. Se uno o più utenti disconnettono le rispettive sessioni Desktop remoto, possono riconnettersi?  
   - Se il servizio continua a negare le connessioni indipendentemente da quanti utenti disconnettono le loro sessioni, vedi [Problema del listener Desktop remoto](#rd-listener-issue).
   - Se il servizio inizia ad accettare di nuovo le connessioni dopo che un determinato numero di utenti si è disconnesso dalle rispettive sessioni, [verifica il criterio relativo al limite di connessioni](#check-the-connection-limit-policy).

### <a name="rdsh-server-memory-issue"></a>Problema di memoria del server Host sessione Desktop remoto

È stata rilevata una perdita di memoria in alcuni server Host sessione Desktop remoto Windows Server 2012 R2. Con il passare del tempo questi server iniziano a rifiutare sia le connessioni Desktop remoto sia gli accessi alla console locale con messaggi simili al seguente:

> Impossibile completare l'attività che si sta tentando di eseguire perché Servizi Desktop remoto è attualmente occupato. Riprovare tra alcuni minuti. È possibile che altri utenti riescano comunque a connettersi.

Anche i client Desktop remoto che tentano di connettersi smettono di rispondere.

Per ovviare a questo problema, riavvia il server Host sessione Desktop remoto.

Per risolvere questo problema, applica l'aggiornamento KB 4093114, [10 aprile 2018 - KB4093114 (rollup mensile)](file:///C:/Users/v-jesits/AppData/Local/Microsoft/Windows/INetCache/Content.Outlook/FUB8OO45/April%2010,%202018—KB4093114%20(Monthly%20Rollup)), ai server Host sessione Desktop remoto.

### <a name="rd-listener-issue"></a>Problema del listener Desktop remoto

In alcuni server Host sessione Desktop remoto aggiornati direttamente da Windows Server 2008 R2 a Windows Server 2012 R2 o Windows Server 2016 è stato rilevato un problema. Quando un client Desktop remoto si connette al server Host sessione Desktop remoto, tale server crea un listener Desktop remoto per la sessione utente. I server interessati effettuano un conteggio dei listener Remote Desktop che aumenta man mano che gli utenti si connettono, ma che non diminuisce mai.

Per ovviare a questo problema, puoi applicare i metodi seguenti:

  - Riavvia il server Host sessione Desktop remoto per reimpostare il conteggio dei listener Desktop remoto.
  - Modifica il criterio relativo al limite di connessioni impostandolo su un valore molto alto. Per altre informazioni sulla gestione di tale criterio, vedi [Verificare il criterio relativo al limite di connessioni](#check-the-connection-limit-policy).

Per risolvere questo problema, applica gli aggiornamenti seguenti ai server Host sessione Desktop remoto:

  - Windows Server 2012 R2: KB 4343891, [30 agosto 2018 - KB4343891 (anteprima del rollup mensile)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884, [30 agosto 2018 - KB4343884 (build sistema operativo 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)

### <a name="check-the-connection-limit-policy"></a>Verificare il criterio relativo al limite di connessioni

Puoi impostare il limite relativo al numero di connessioni Desktop remoto simultanee a livello del singolo computer o configurando un oggetto Criteri di gruppo. Per impostazione predefinita, questo limite non è configurato.

Per verificare le impostazioni correnti e identificare gli eventuali oggetti Criteri di gruppo esistenti nel server Host sessione Desktop remoto, apri una finestra del prompt dei comandi come amministratore e immetti il comando seguente:
  
```
gpresult /H c:\gpresult.html
```
   
Al termine di questo comando, apri gpresult.html e in **Configurazione computer\\Modelli amministrativi\\Componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\Connessioni** trova il criterio **Limita il numero di connessioni**.

  - Se l'impostazione di questo criterio è **Disabilitato**, i criteri di gruppo non limitano le connessioni RDP.
  - Se l'impostazione di questo criterio è **Abilitato**, controlla **Oggetto Criteri di gruppo dominante**. Se devi rimuovere o modificare il limite relativo alle connessioni, modifica questo oggetto Criteri di gruppo.

Per applicare le modifiche apportate al criterio, apri una finestra del prompt dei comandi nel computer interessato e immetti il comando seguente:
  
```
gpupdate /force
```
  
## <a name="rd-client-disconnects-and-cannot-reconnect-to-the-same-session"></a>Il client Desktop remoto si disconnette e non riesce a riconnettersi alla stessa sessione

Dopo aver perso la connessione al desktop remoto, il client Desktop remoto non riesce immediatamente a riconnettersi. L'utente riceve messaggi di errore simili ai seguenti:

  - Impossibile connettersi al server terminal. Errore di sicurezza. Dopo aver verificato di essere connessi alla rete, provare a riconnettersi al server.
  - Desktop remoto disconnesso. Impossibile connettersi al computer remoto. Errore di sicurezza. Verificare di essere connessi alla rete, quindi provare a di nuovo a connettersi.

Successivamente il client Desktop remoto si riconnette al desktop remoto. Tuttavia, invece di connettere il client alla sessione originaria, il server Host sessione Desktop remoto connette il client a una nuova sessione. Se esamini il server Host sessione Desktop remoto, risulta chiaro che la sessione originaria non è passata allo stato disconnesso, ma rimane attiva.

Per ovviare a questo problema, puoi abilitare il criterio **Configura intervallo keep-alive della connessione** nella cartella di Criteri di gruppo **Configurazione computer\\Modelli amministrativi\\Componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\Connessioni**. Se abiliti questo criterio, devi immettere un intervallo keep-alive. Tale intervallo determina con quale frequenza, in minuti, il server verifica lo stato della sessione.

Questo problema può essere causato da una configurazione errata delle impostazioni di autenticazione e configurazione. Puoi configurare tali impostazioni a livello di server o mediante oggetti Criteri di gruppo. Le impostazioni dei criteri di gruppo sono disponibili nella cartella di Criteri di gruppo **Configurazione computer\\Modelli amministrativi\\Componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\Sicurezza**.

1. Aprire Configurazione host sessione Desktop remoto sul server Host sessione Desktop remoto.
2. In **Connessioni** fai clic con il pulsante destro del mouse sul nome della connessione e quindi scegli **Proprietà**.
3. Nella finestra di dialogo **Proprietà** per la connessione, nella scheda **Generale**, nel livello **Sicurezza** seleziona un metodo di sicurezza.
4. In **Livello di crittografia** fai clic sul livello desiderato. Puoi selezionare **Basso**, **Compatibile con client**, **Alto** o **Conforme FIPS**.

> [!NOTE]  
>  - Se le comunicazioni tra i client e i server Host sessione Desktop remoto richiedono il livello di crittografia più alto, usa la crittografia Conforme FIPS.
>  - Qualunque impostazione del livello di crittografia configurato in Criteri di gruppo esegue l'override della configurazione impostata usando lo strumento Configurazione di Servizi Desktop remoto. Inoltre, se abiliti il criterio [Crittografia di sistema: utilizza algoritmi FIPS compatibili per crittografia, hash e firma](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc780081\(v=ws.10\)), questa impostazione esegue l'override del criterio **Imposta livello di crittografia connessione client**. Il criterio di crittografia di sistema è disponibile nella cartella **Configurazione computer\\Impostazioni di Windows\\Impostazioni sicurezza\\Criteri locali\\Opzioni di sicurezza**.
>  - Quando si modifica il livello di crittografia, il nuovo valore impostato viene applicato al successivo accesso di un utente. Se sono necessari più livelli di crittografia in un server, installa più schede di rete e configura ciascuna scheda separatamente.
>  - Per verificare che il certificato abbia una chiave privata corrispondente, in Configurazione di Servizi Desktop remoto fai clic con il pulsante destro del mouse sulla connessione per la quale vuoi visualizzare il certificato, scegli **Generale**, fai clic su **Modifica**, seleziona il certificato da esaminare e infine fai clic su **Visualizza certificato**. Nella parte inferiore della scheda **Generale** deve venire visualizzata la dicitura "L'utente possiede una chiave privata corrispondente al certificato". Puoi visualizzare queste informazioni anche usando lo snap-in Certificati.
>  - La crittografia Conforme FIPS (il criterio **Crittografia di sistema: utilizza algoritmi FIPS compatibili per crittografia, hash e firma** o l'impostazione **Conforme FIPS** in Configurazione Server Desktop remoto) crittografa e decrittografa i dati inviati dal client al server e dal server al client con gli algoritmi di crittografia FIPS (Federal Information Processing Standard) 140-1 usando moduli crittografici Microsoft. Per altre informazioni, vedi [Convalida FIPS 140](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation).
>  - L'impostazione **Alto** crittografa i dati inviati dal client al server e dal server al client usando la crittografia avanzata a 128 bit.
>  - L'impostazione **Compatibile con client** crittografa i dati inviati tra il client e il server con il massimo livello di sicurezza della chiave supportato dal client.
>  - L'impostazione **Basso** crittografa i dati inviati dal client al server usando la crittografia a 56 bit.

## <a name="remote-laptop-disconnects-from-wireless-network"></a>I portatili remoti si disconnettono dalla rete wireless

Questo problema può verificarsi quando un client Desktop remoto si connette a un portatile mediante una rete wireless 802.1x. A intermittenza il portatile si disconnette dalla rete wireless e non si riconnette automaticamente.

Si tratta di un problema noto che si verifica quando l'impostazione di autenticazione di rete per la connessione di rete wireless è **Autenticazione utente**.

Per ovviare a questo problema, configura l'impostazione di autenticazione di rete su **Autenticazione utente o computer** oppure su **Autenticazione computer**.

 > [!NOTE]  
> Per modificare le impostazioni di autenticazione di rete in un singolo computer, potresti dover usare il pannello di controllo Centro connessioni di rete e condivisione per creare una nuova connessione wireless con le nuove impostazioni.

Per una descrizione completa di come configurare le impostazioni di rete wireless mediante oggetti Criteri di gruppo, vedi [Configurare criteri di rete wireless (IEEE 802.11)](https://docs.microsoft.com/windows-server/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies).

## <a name="user-experiences-poor-performance-or-application-problems"></a>L'utente riscontra prestazioni non soddisfacenti o problemi con le applicazioni

Questa sezione tratta diversi problemi comuni che gli utenti possono incontrare quando usano la funzionalità Desktop remoto:

  - [Gli utenti che si connettono a nuove macchine virtuali di Microsoft Azure possono avere problemi intermittenti](#intermittent-problems-with-new-microsoft-azure-virtual-machines).
  - [Gli utenti che si connettono a desktop remoti Windows 10 versione 1709 possono avere problemi durante la riproduzione di video](#video-playback-issues-on-windows-10-version-1709).
  - [Gli utenti con profili utente di sola lettura che si connettono a desktop remoti Windows 10 non riescono a condividere i loro desktop.](#desktop-sharing-issues-on-windows-10)
  - [Gli utenti che si connettono a desktop remoti Windows 10 usando client che eseguono versioni meno recenti di Windows 10 riscontrano scarse prestazioni se l'Autenticazione a livello di rete è disabilitata.](#performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled)
  - [Quando gli utenti eseguono più applicazioni nelle sessioni Desktop remoto e si disconnettono e riconnettono frequentemente, alla riconnessione può venire visualizzata una schermata nera.](#black-screen-issue)

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problemi intermittenti con le nuove macchine virtuali di Microsoft Azure

Questo problema riguarda le macchine virtuali di cui è stato effettuato di recente il provisioning. Dopo che l'utente si connette alla macchina virtuale, la sessione Desktop remoto potrebbe non caricare correttamente tutte le impostazioni dell'utente.

Per ovviare a questo problema, disconnettiti dalla macchina virtuale, attendi almeno 20 minuti e quindi connettiti di nuovo.

Per risolvere questo problema, applica gli aggiornamenti seguenti alle macchine virtuali a seconda dei casi:

  - Windows 10 e Windows Server 2016: KB 4343884, [30 agosto 2018 - KB4343884 (build sistema operativo 14393.2457)](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891, [30 agosto 2018 - KB4343891 (anteprima del rollup mensile)](https://support.microsoft.com/en-us/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problemi di riproduzione video in Windows 10 versione 1709

Questo problema si verifica quando gli utenti si connettono a computer remoti che eseguono Windows 10, versione 1709. Se tali utenti riproducono video usando il codec VMR9 (Video Mixing Renderer 9), il lettore mostra solo una finestra nera.

Si tratta di un problema noto in Windows 10, versione1709. Il problema non si verifica in Windows 10 versione 1703.

### <a name="desktop-sharing-issues-on-windows-10"></a>Problemi di condivisione del desktop in Windows 10

Questo problema si verifica quando l'utente ha un profilo utente di sola lettura (e un hive del Registro di sistema associato), come nel caso di uno scenario con un chiosco multimediale. Se tale utente si connette a un computer remoto che esegue Windows 10 versione 1803, non riesce a condividere il proprio desktop.

Per risolvere questo problema, applica l'aggiornamento 4340917 di Windows 10, [24 luglio 2018 - KB4340917 (build sistema operativo 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problemi di prestazioni quando vengono usate insieme versioni diverse di Windows 10 se l'Autenticazione a livello di rete è disabilitata

Questo problema si verifica quando l'Autenticazione a livello di rete è disabilitata e i computer client Desktop remoto che eseguono Windows 10 si connettono a desktop remoti che eseguono versioni diverse di Windows 10. Gli utenti dei client Desktop remoto nei computer che eseguono Windows 10 versione 1709 o versioni precedenti riscontrano prestazioni scarse quando si connettono a desktop remoti che eseguono Windows 10 versione 1803 o versioni successive.

Ciò si verifica perché, se l'Autenticazione a livello di rete è disabilitata, i computer client meno recenti usano un protocollo più lento per connettersi a Windows 10 versione 1803 o a una versione successiva.

Per risolvere questo problema, applica l'aggiornamento KB 4340917, [24 luglio 2018 - KB4340917 (build sistema operativo 17134.191)](https://support.microsoft.com/en-us/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Problema della schermata nera

Questo problema si verifica in Windows 8.0, Windows 8.1, Windows 10 RTM e Windows Server 2012 R2. Un utente avvia più applicazioni in un desktop remoto e quindi si disconnette dalla sessione. L'utente poi si riconnette periodicamente al desktop remoto per interagire con le applicazioni e quindi si disconnette di nuovo. A un certo punto, quando l'utente si riconnette, la sessione Desktop remoto mostra solo una schermata nera. L'utente deve terminare la sessione Desktop remoto dalla console del computer remoto (o dalla console del server Host sessione Desktop remoto). Questa azione interrompe l'esecuzione delle applicazioni.

Per risolvere questo problema, applica gli aggiornamenti seguenti a seconda dei casi:

  - Windows 8 e Windows Server 2012: KB 4103719, [17 maggio 2018 - KB4103719 (anteprima del rollup mensile)](https://support.microsoft.com/en-us/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 e Windows Server 2012 R2: KB 4103724, [17 maggio 2018 - KB4103724 (anteprima del rollup mensile)](https://support.microsoft.com/en-us/help/4103724/windows-81-update-kb4103724) e KB 4284863, [21 giugno 2018 - KB4284863 (anteprima del rollup mensile)](https://support.microsoft.com/en-us/help/4284863/windows-81-update-kb4284863)
  - Windows 10: corretto in KB 4284860, [12 giugno 2018 - KB4284860 (build sistema operativo 10240.17889)](https://support.microsoft.com/en-us/help/4284860/windows-10-update-kb4284860)
