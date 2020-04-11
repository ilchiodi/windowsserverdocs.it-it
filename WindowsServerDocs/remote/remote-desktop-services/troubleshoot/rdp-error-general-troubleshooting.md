---
title: Risoluzione dei problemi relativi alle connessioni Desktop remoto
description: Risoluzione dei problemi relativi alla mancata registrazione di una classe con una connessione Desktop remoto.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 03c3c8daa8dc4bea0e03ed285a98401f91cdf1cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857214"
---
# <a name="general-remote-desktop-connection-troubleshooting"></a>Risoluzione dei problemi relativi alle connessioni Desktop remoto

Segui questa procedura quando un client Desktop remoto non riesce a connettersi a un desktop remoto, ma non fornisce messaggi o altri sintomi utili per identificare la causa.

## <a name="check-the-status-of-the-rdp-protocol"></a>Verificare lo stato del protocollo RDP

### <a name="check-the-status-of-the-rdp-protocol-on-a-local-computer"></a>Verificare lo stato del protocollo RDP in un computer locale

Per verificare e modificare lo stato del protocollo RDP in un computer locale, vedi [Come abilitare Desktop remoto](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access#how-to-enable-remote-desktop).

> [!NOTE]  
> Se le opzioni Desktop remoto non sono disponibili, vedi [Verificare se un oggetto Criteri di gruppo blocca RDP](#check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer).

### <a name="check-the-status-of-the-rdp-protocol-on-a-remote-computer"></a>Verificare lo stato del protocollo RDP in un computer remoto

> [!IMPORTANT]  
> Segui attentamente le istruzioni della sezione. Un'errata modifica del Registro di sistema può causare gravi problemi. Prima di iniziare a modificare il Registro di sistema, [eseguine il backup](https://support.microsoft.com/help/322756) in modo da poterlo ripristinare in caso di problemi.

Per verificare e modificare lo stato del protocollo RDP in un computer remoto, usa una connessione al Registro di sistema in rete:

1. Prima di tutto, vai al menu **Start** e scegli **Esegui**. Nella casella di testo visualizzata immetti **regedt32**.
2. Nell'editor del Registro di sistema scegli **Connetti a Registro di sistema in rete** dal menu **File**.
3. Nella finestra di dialogo **Seleziona computer** immetti il nome del computer remoto, fai clic su **Controlla nomi** e quindi su **OK**.
4. Passa a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**.  
   ![Editor del Registro di sistema, in cui viene mostrata la voce fDenyTSConnections](../media/troubleshoot-remote-desktop-connections/RegEntry_fDenyTSConnections.png)
   - Se il valore della chiave **fDenyTSConnections** è **0**, RDP è abilitato.
   - Se il valore della chiave **fDenyTSConnections** è **1**, RDP è disabilitato.
5. Per abilitare RDP, modifica il valore di **fDenyTSConnections** sostituendo **1** con **0**.

### <a name="check-whether-a-group-policy-object-gpo-is-blocking-rdp-on-a-local-computer"></a>Verificare se un oggetto Criteri di gruppo blocca RDP in un computer locale

Se non riesci ad abilitare RDP nell'interfaccia utente o se il valore di **fDenyTSConnections** viene reimpostato su **1** dopo che è stato modificato, è possibile che un oggetto Criteri di gruppo esegua l'override delle impostazioni a livello di computer.

Per verificare la configurazione di Criteri di gruppo in un computer locale, apri una finestra del prompt dei comandi come amministratore e immetti il comando seguente:

```cmd
gpresult /H c:\gpresult.html
```

Al termine del comando, apri gpresult.html. In **Configurazione computer\\Modelli amministrativi\\Componenti di Windows\\Servizi Desktop remoto\\Host sessione Desktop remoto\\Connessioni** trova il criterio **Consenti la connessione remota tramite Servizi Desktop remoto**.

- Se l'impostazione di questo criterio è **Abilitato**, i criteri di gruppo non bloccano le connessioni RDP.
- Se l'impostazione di questo criterio è **Disabilitato**, controlla **Oggetto Criteri di gruppo dominante**. Questo è l'oggetto Criteri di gruppo che blocca le connessioni RDP.
  ![Segmento di esempio di gpresult.html, in cui l'oggetto Criteri di gruppo a livello di dominio **Block RDP** disabilita RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_GP.png)
   
  ![Segmento di esempio di gpresult.html, in cui **Local Group Policy** disabilita RDP.](../media/troubleshoot-remote-desktop-connections/GPResult_RDSH_Connections_LGP.png)

### <a name="check-whether-a-gpo-is-blocking-rdp-on-a-remote-computer"></a>Verificare se un oggetto Criteri di gruppo blocca RDP in un computer remoto

Per verificare la configurazione di Criteri di gruppo in un computer remoto, il comando è quasi identico a quello per un computer locale:

```cmd
gpresult /S <computer name> /H c:\gpresult-<computer name>.html
```

Il file prodotto da questo comando (**gpresult-\<nome computer\>.html**) usa per le informazioni lo stesso formato usato dalla versione per il computer locale (**gpresult.html**).

### <a name="modifying-a-blocking-gpo"></a>Modifica di un oggetto Criteri di gruppo che blocca

Puoi modificare queste impostazioni nell'Editor oggetti Criteri di gruppo e nella Console Gestione Criteri di gruppo. Per altre informazioni su come usare i criteri di gruppo, vedi [Gestione avanzata di Criteri di gruppo](https://docs.microsoft.com/microsoft-desktop-optimization-pack/agpm/).

Per modificare il criterio che blocca, usa uno dei metodi seguenti:

- Nell'Editor oggetti Criteri di gruppo accedi al livello appropriato di oggetti Criteri di gruppo (ad esempio, locali o di dominio) e passa a **Configurazione computer** > **Modelli amministrativi** > **Componenti di Windows** > **Servizi Desktop remoto** > **Host sessione Desktop remoto** > **Connessioni** > **Consenti la connessione remota tramite Servizi Desktop remoto**.  
   1. Imposta il criterio su **Abilitato** o **Non configurato**.
   2. Nei computer interessati apri una finestra del prompt dei comandi come amministratore ed esegui il comando **gpupdate /force**.
- Nella Console Gestione Criteri di gruppo passa all'unità organizzativa (OU) in cui il criterio che blocca è applicato ai computer interessati ed elimina tale criterio dall'unità organizzativa.

## <a name="check-the-status-of-the-rdp-services"></a>Verificare lo stato dei servizi RDP

Sia nel computer (client) locale sia nel computer (di destinazione) remoto devono essere in esecuzione i servizi seguenti:

- Servizi Desktop remoto (TermService)
- Redirector porta UserMode di Servizi Desktop remoto (UmRdpService)

Puoi usare lo snap-in Servizi di MMC per gestire i servizi in locale o in remoto. Puoi anche usare PowerShell per gestire i servizi in locale o in remoto se il computer remoto è configurato in modo da accettare cmdlet di PowerShell remoti.

![Servizi di Desktop remoto nello snap-in Servizi di MMC. Non modificare le impostazioni predefinite dei servizi.](../media/troubleshoot-remote-desktop-connections/RDSServiceStatus.png)

In tutti e due i computer, se uno o entrambi i servizi non sono in esecuzione, avviali.

> [!NOTE]  
> Se avvii Servizi Desktop remoto, fai clic su **Sì** per riavviare automaticamente il servizio Redirector porta UserMode di Servizi Desktop remoto.

## <a name="check-that-the-rdp-listener-is-functioning"></a>Verificare che il listener RDP sia funzionante

> [!IMPORTANT]  
> Segui attentamente le istruzioni della sezione. Un'errata modifica del Registro di sistema può causare gravi problemi. Prima di iniziare a modificare il Registro di sistema, [eseguine il backup](https://support.microsoft.com/help/322756) in modo da poterlo ripristinare in caso di problemi.

### <a name="check-the-status-of-the-rdp-listener"></a>Verificare lo stato del listener RDP

Per questa procedura, usa un'istanza di PowerShell con autorizzazioni amministrative. Per un computer locale, puoi anche usare un prompt dei comandi con autorizzazioni amministrative. In questa procedura viene tuttavia usato PowerShell perché gli stessi cmdlet funzionano sia in locale che in remoto.

1. Per connetterti a un computer remoto, esegui il cmdlet seguente:

   ```powershell
   Enter-PSSession -ComputerName <computer name>
   ```

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
   1. Per eseguire il backup della voce esistente del Registro di sistema, immetti il cmdlet seguente:  
   
      ```powershell  
      cmd /c 'reg export "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp" C:\Rdp-tcp-backup.reg'   
      ```  
   

   2. Per rimuovere la voce esistente del Registro di sistema, immetti i cmdlet seguenti:  
   
      ```powershell  
      Remove-Item -path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-tcp' -Recurse -Force  
      ```
   
   3. Per importare la nuova voce del Registro di sistema e quindi riavviare il servizio, immetti i cmdlet seguenti:  
   
      ```powershell  
      cmd /c 'regedit /s c:\<filename>.reg'  
      Restart-Service TermService -Force  
      ```
      
      Sostituisci \<filename\> con il nome del file con estensione reg esportato.

6. Testa la connessione provando a eseguire di nuovo la connessione al desktop remoto. Se ancora non riesci a connetterti, riavvia il computer interessato.
7. Se il problema persiste, [verifica lo stato del certificato autofirmato RDP](#check-the-status-of-the-rdp-self-signed-certificate).

### <a name="check-the-status-of-the-rdp-self-signed-certificate"></a>Verificare lo stato del certificato autofirmato RDP

1. Se ti è ancora impossibile eseguire la connessione, apri lo snap-in Certificati di MMC. Quando ti viene chiesto di selezionare l'archivio certificati da gestire, seleziona **Account del computer** e quindi il computer interessato.
2. Nella sottocartella **Certificati** di **Desktop remoto** elimina il certificato autofirmato RDP. 
    ![Certificati di Desktop remoto nello snap-in Certificati di MMC.](../media/troubleshoot-remote-desktop-connections/MMCCert_Delete.png)
3. Nel computer interessato riavvia Servizi Desktop remoto.
4. Aggiorna lo snap-in Certificati.
5. Se il certificato autofirmato RDP non è stato ricreato, [verifica le autorizzazioni della cartella MachineKeys](#check-the-permissions-of-the-machinekeys-folder).

### <a name="check-the-permissions-of-the-machinekeys-folder"></a>Verificare le autorizzazioni della cartella MachineKeys

1. Nel computer interessato apri Esplora risorse e quindi passa a **C:\\ProgramData\\Microsoft\\Crypto\\RSA\\** .
2. Fai clic con il pulsante destro del mouse su **MachineKeys**, scegli **Proprietà**, fai clic su **Sicurezza** e quindi su **Avanzate**.
3. Assicurati che siano configurate le autorizzazioni seguenti:
      - Builtin\\Administrators: Controllo completo
      - Everyone: Lettura, Scrittura

## <a name="check-the-rdp-listener-port"></a>Verificare la porta del listener RDP

Sia nel computer (client) locale sia nel computer (di destinazione) remoto il listener RDP deve essere in ascolto sulla porta 3389. Nessun'altra applicazione deve usare questa porta.

> [!IMPORTANT]  
> Segui attentamente le istruzioni della sezione. Un'errata modifica del Registro di sistema può causare gravi problemi. Prima di iniziare a modificare il Registro di sistema, [eseguine il backup](https://support.microsoft.com/help/322756) in modo da poterlo ripristinare in caso di problemi.

Per verificare o modificare la porta RDP, usa l'editor del Registro di sistema:

1. Vai al menu Start, scegli **Esegui** e quindi immetti **regedt32** nella casella di testo visualizzata.
      - Per eseguire la connessione a un computer remoto, scegli **Connetti a Registro di sistema in rete** dal menu **File**.
      - Nella finestra di dialogo **Seleziona computer** immetti il nome del computer remoto, fai clic su **Controlla nomi** e quindi su **OK**.
2. Apri il Registro di sistema e passa a **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<listener\>** . 
    ![Sottochiave PortNumber per il protocollo RDP.](../media/troubleshoot-remote-desktop-connections/RegEntry_PortNumber.png)
3. Se **PortNumber** ha un valore diverso da **3389**, impostalo su **3389**. 
   > [!IMPORTANT]  
    > Puoi far funzionare i servizi di Desktop remoto usando un'altra porta. Questa operazione non è tuttavia consigliata. Questo articolo non illustra come risolvere i problemi relativi a questo tipo di configurazione.
4. Dopo aver modificato il numero di porta, riavvia Servizi Desktop remoto.

### <a name="check-that-another-application-isnt-trying-to-use-the-same-port"></a>Verificare che un'altra applicazione non stia provando a usare la stessa porta

Per questa procedura, usa un'istanza di PowerShell con autorizzazioni amministrative. Per un computer locale, puoi anche usare un prompt dei comandi con autorizzazioni amministrative. In questa procedura viene tuttavia usato PowerShell perché gli stessi cmdlet funzionano sia in locale che in remoto.

1. Aprire una finestra di PowerShell. Per eseguire la connessione a un computer remoto, immetti **Enter-PSSession -ComputerName \<nome computer\>** .
2. Immettere il comando seguente:  
   
     ```powershell  
    cmd /c 'netstat -ano | find "3389"'  
    ```
  
    ![Il comando netstat genera un elenco di porte e dei servizi in ascolto su di esse.](../media/troubleshoot-remote-desktop-connections/WPS_netstat.png)
3. Cercare una voce per la porta TCP 3389 (o la porta RDP assegnata) con lo stato **In ascolto**. 
    > [!NOTE]  
   > L'identificatore (PID) del processo o del servizio che usa la porta è visualizzato nella colonna PID.
4. Per determinare quale applicazione usa la porta 3389 (o la porta RDP assegnata), immetti il comando seguente:  
   
     ```powershell  
    cmd /c 'tasklist /svc | find "<pid listening on 3389>"'  
    ```  
  
    ![Il comando tasklist fornisce i dettagli di un processo specifico.](../media/troubleshoot-remote-desktop-connections/WPS_tasklist.png)
5. Cerca una voce per il numero PID associato alla porta (fornito dall'output di **netstat**). I servizi o i processi associati a tale PID vengono visualizzati nella colonna destra.
6. Se la porta viene usata da un'applicazione o da un servizio diverso da Servizi Desktop remoto (TermServ.exe), puoi risolvere il conflitto usando uno dei metodi seguenti:
      - Configura l'altra applicazione o l'altro servizio in modo che usi una porta diversa (scelta consigliata).
      - Disinstalla l'altra applicazione o l'altro servizio.
      - Configura RDP in modo che usi una porta diversa e quindi riavvia Servizi Desktop remoto (scelta non consigliata).

### <a name="check-whether-a-firewall-is-blocking-the-rdp-port"></a>Verificare se la porta RDP è bloccata da un firewall

Usa lo strumento **psping** per verificare se puoi raggiungere il computer interessato tramite la porta 3389.

1. Passa a un computer diverso che non è interessato dal problema e scarica **psping** da <https://live.sysinternals.com/psping.exe>.
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
