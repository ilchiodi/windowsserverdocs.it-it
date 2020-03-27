---
title: Configurare il server di accesso remoto per VPN Always On
description: RRAS è progettato per essere eseguito correttamente sia come router che come server di accesso remoto. Pertanto, supporta una vasta gamma di funzionalità.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: lizross
author: eross-msft
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 9d3afb21c466ef1010a20ec811df45b9dcb2b711
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312259"
---
# <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>Passaggio 3. Configurare il server di accesso remoto per VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 2. Configurare l'infrastruttura server](vpn-deploy-server-infrastructure.md)
- [**Precedente:** Passaggio 4. Installare e configurare il server dei criteri di rete](vpn-deploy-nps.md)

RRAS è progettato per essere eseguito correttamente sia come router che come server di accesso remoto, perché supporta un'ampia gamma di funzionalità. Ai fini di questa distribuzione, è necessario solo un piccolo subset di queste funzionalità: supporto per le connessioni VPN IKEv2 e il routing LAN.

IKEv2 è un protocollo di tunneling VPN descritto in Internet Engineering Task Force Request per i commenti 7296. Il vantaggio principale di IKEv2 è che tollera le interruzioni nella connessione di rete sottostante. Se ad esempio la connessione viene persa temporaneamente o un utente sposta un computer client da una rete a un'altra, IKEv2 ripristina automaticamente la connessione VPN quando viene ristabilita la connessione di rete, senza l'intervento dell'utente.

Configurare il server RRAS per supportare le connessioni IKEv2 e disabilitare i protocolli inutilizzati, riducendo il footprint di sicurezza del server. Inoltre, configurare il server per l'assegnazione di indirizzi ai client VPN da un pool di indirizzi statici. È possibile tenerne assegnare gli indirizzi da un pool o da un server DHCP. Tuttavia, l'utilizzo di un server DHCP aggiunge complessità alla progettazione e offre vantaggi minimi.

>[!IMPORTANT]
>È importante:
>- Installare due schede di rete Ethernet nel server fisico. Se si sta installando il server VPN in una macchina virtuale, è necessario creare due commutatori virtuali esterni, uno per ogni scheda di rete fisica. quindi creare due schede di rete virtuali per la macchina virtuale, con ogni scheda di rete connessa a un comunicatore virtuale.
>
>- Installare il server nella rete perimetrale tra i firewall perimetrali e interni, con una scheda di rete connessa alla rete perimetrale esterna e una scheda di rete connessa alla rete perimetrale interna.

>[!WARNING]
>Prima di iniziare, assicurarsi di abilitare IPv6 nel server VPN. In caso contrario, non è possibile stabilire una connessione e viene visualizzato un messaggio di errore.

## <a name="install-remote-access-as-a-ras-gateway-vpn-server"></a>Installare accesso remoto come server VPN gateway RAS

In questa procedura viene installato il ruolo accesso remoto come server VPN del gateway RAS a tenant singolo. Per altre informazioni, vedi [Accesso remoto](../../../Remote-Access.md).

### <a name="install-the-remote-access-role-by-using-windows-powershell"></a>Installare il ruolo accesso remoto usando Windows PowerShell

1. Aprire Windows PowerShell come **amministratore**.

2. Immettere ed eseguire il cmdlet seguente:

   ```powershell
   Install-WindowsFeature DirectAccess-VPN -IncludeManagementTools
   ```

   Al termine dell'installazione, viene visualizzato il messaggio seguente in Windows PowerShell.

   ```powershell
   | Success | Restart Needed | Exit Code |               Feature Result               |
   |---------|----------------|-----------|--------------------------------------------|
   |  True   |       No       |  Success  | {RAS Connection Manager Administration Kit |
   ```

### <a name="install-the-remote-access-role-by-using-server-manager"></a>Installare il ruolo accesso remoto usando Server Manager

È possibile utilizzare la procedura seguente per installare il ruolo accesso remoto utilizzando Server Manager.

1. Nel server VPN, in Server Manager, selezionare **Gestisci** e selezionare **Aggiungi ruoli e funzionalità**.
   
   Viene avviata l'Aggiunta guidata ruoli e funzionalità.

2. Nella pagina prima di iniziare selezionare **Avanti**.

3. Nella pagina Selezione tipo di installazione selezionare l'opzione **installazione basata su ruoli o basata su funzionalità** e quindi fare clic su **Avanti**.

4. Nella pagina Selezione server di destinazione selezionare l'opzione **selezionare un server dal pool di server** .

5. In pool di server selezionare il computer locale e fare clic su **Avanti**.

6. Nella pagina Selezione ruoli server, in **ruoli**, selezionare **accesso remoto**e quindi **Avanti**.

7. Nella pagina Selezione funzionalità selezionare **Avanti**.

8. Nella pagina accesso remoto selezionare **Avanti**.

9.  Nella pagina Selezione servizio ruolo, in **servizi ruolo**, selezionare **DirectAccess e VPN (RAS)** .

   Verrà visualizzata la finestra di dialogo **Aggiunta guidata ruoli e funzionalità** .

11. Nella finestra di dialogo Aggiungi ruoli e funzionalità selezionare **Aggiungi funzionalità** e quindi fare clic su **Avanti**.

12. Nella pagina ruolo server Web (IIS) fare clic su **Avanti**.

13. Nella pagina Selezione servizi ruolo selezionare **Avanti**.

14. Nella pagina Conferma selezioni per l'installazione rivedere le scelte effettuate e quindi selezionare **Installa**.

15. Al termine dell'installazione, fare clic su **Chiudi**.

## <a name="configure-remote-access-as-a-vpn-server"></a>Configurare l'accesso remoto come server VPN

In questa sezione è possibile configurare la VPN di accesso remoto per consentire le connessioni VPN IKEv2, negare le connessioni da altri protocolli VPN e assegnare un pool di indirizzi IP statici per il rilascio degli indirizzi IP per la connessione dei client VPN autorizzati.

1. Nel server VPN, in Server Manager, selezionare il flag **notifiche** .

2. Nel menu **attività** selezionare **apri procedura guidata Introduzione**

   Verrà visualizzata la configurazione guidata accesso remoto.

   >[!NOTE]
   >La configurazione guidata accesso remoto potrebbe essere visualizzata dietro Server Manager. Se si ritiene che la procedura guidata stia impiegando troppo tempo per l'apertura, lo spostamento o la riduzione del Server Manager per verificare se la procedura guidata è alla base. In caso contrario, attendere l'inizializzazione della procedura guidata.

3. Selezionare **Distribuisci solo VPN**.

    Si apre Microsoft Management Console (MMC) di routing e accesso remoto.

4. Fare clic con il pulsante destro del mouse sul server VPN, quindi scegliere **Configura e Abilita routing e accesso remoto**.

   Si apre l'installazione guidata del server Routing e accesso remoto.

5. Nella pagina iniziale dell'installazione guidata del server di routing e accesso remoto selezionare **Avanti**.

6. In **configurazione**selezionare **configurazione personalizzata**e quindi fare clic su **Avanti**.

7. In **configurazione personalizzata**selezionare **accesso VPN**e quindi fare clic su **Avanti**.

   Viene avviata l'installazione guidata del server di routing e accesso remoto.

8. Selezionare **fine** per chiudere la procedura guidata, quindi fare clic su **OK** per chiudere la finestra di dialogo Routing e accesso remoto.

9. Selezionare **Avvia servizio** per avviare l'accesso remoto.

10. In MMC di accesso remoto fare clic con il pulsante destro del mouse sul server VPN, quindi scegliere **Proprietà**.

11. In Proprietà selezionare la scheda **sicurezza** e procedere come segue:

    a. Selezionare **provider di autenticazione** e selezionare **autenticazione RADIUS**.

    b. Selezionare **Configura**.

       Verrà visualizzata la finestra di dialogo autenticazione RADIUS.

    c. Fare clic su **Aggiungi**.

       Verrà visualizzata la finestra di dialogo Aggiungi server RADIUS.

    d. In **nome server**immettere il nome di dominio completo (FQDN) del server dei criteri di rete nella propria organizzazione/rete aziendale.
    
       Se ad esempio il nome NetBIOS del server NPS è NPS1 e il nome di dominio è corp.contoso.com, immettere **nps1.Corp.contoso.com**.

    e. In **segreto condiviso**selezionare **Cambia**.

       Verrà visualizzata la finestra di dialogo Cambia segreto.

    f. In **nuovo segreto**immettere una stringa di testo.

    g. In **Conferma nuovo segreto**immettere la stessa stringa di testo e quindi fare clic su **OK**.

    >[!IMPORTANT]
    >Salvare la stringa di testo. Quando si configura il server dei criteri di rete nella propria organizzazione/rete aziendale, questo server VPN verrà aggiunto come client RADIUS. Durante questa configurazione, si utilizzerà lo stesso segreto condiviso per consentire ai server dei criteri di rete e VPN di comunicare.

12. In **Aggiungi server RADIUS**esaminare le impostazioni predefinite per:

    - **Timeout**

    - **Punteggio iniziale**

    - **Porta**

13. Se necessario, modificare i valori in modo che corrispondano ai requisiti per l'ambiente e selezionare **OK**.

    Un NAS è un dispositivo che fornisce un certo livello di accesso a una rete più ampia. Un NAS che usa un'infrastruttura RADIUS è anche un client RADIUS, che invia le richieste di connessione e i messaggi di contabilità a un server RADIUS per l'autenticazione, l'autorizzazione e l'accounting.

14. Esaminare l'impostazione per il **provider di contabilità**:

    |                    Se si desidera...                     |                                                     Then…                                                      |
    |-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
    | Attività di accesso remoto registrata nel server di accesso remoto |                               Assicurarsi che l' **Accounting di Windows** sia selezionato.                               |
    |        Server dei criteri di rete per eseguire servizi di contabilità per VPN         | Modificare **provider contabilità** in **Accounting RADIUS** e quindi configurare il server dei criteri di server come provider contabilità. |

15. Selezionare la scheda **IPv4** ed eseguire le operazioni seguenti:

    a. Selezionare **pool di indirizzi statici**.

    b. Selezionare **Aggiungi** per configurare un pool di indirizzi IP.

       Il pool di indirizzi statici deve contenere indirizzi della rete perimetrale interna. Questi indirizzi si trovano nella connessione di rete interna nel server VPN e non nella rete aziendale.

    c. In **indirizzo IP iniziale**immettere l'indirizzo IP iniziale nell'intervallo che si vuole assegnare ai client VPN.

    d. In **indirizzo IP finale**, immettere l'indirizzo IP finale nell'intervallo da assegnare ai client VPN oppure, in **numero di indirizzi**, immettere il numero dell'indirizzo che si vuole rendere disponibile. Se si usa DHCP per questa subnet, assicurarsi di configurare un'esclusione degli indirizzi corrispondente nei server DHCP.

    e. Opzionale Se si utilizza DHCP, selezionare **Adapter**e nell'elenco dei risultati selezionare la scheda Ethernet connessa alla rete perimetrale interna.

16. Opzionale *Se si configura l'accesso condizionale per la connettività VPN*, nell'elenco a discesa **certificato** , in **associazione certificato SSL**, selezionare l'autenticazione del server VPN.

17. Opzionale *Se si configura l'accesso condizionale per la connettività VPN*, in MMC Server dei criteri di **rete espandere criteri\\criteri di rete** e procedere come segue: 

    a. Right-le **connessioni ai criteri di rete del server Microsoft Routing e accesso remoto** e selezionare **Proprietà**.

    b. Selezionare la **concessione dell'accesso. Concedere l'accesso se la richiesta di connessione corrisponde a questa opzione dei criteri** .

    c. In tipo di server di accesso alla rete selezionare **server di accesso remoto (VPN-connessione remota)** nell'elenco a discesa.

18. In MMC Routing e accesso remoto, fare clic con il pulsante destro del mouse su **porte,** quindi scegliere **Proprietà**. 
    
    Verrà visualizzata la finestra di dialogo Proprietà porte.

19. Selezionare **WAN Miniport (SSTP)** e selezionare **Configura**. Verrà visualizzata la finestra di dialogo Configura dispositivo-WAN Miniport (SSTP).

    a. Deselezionare le caselle di controllo **connessioni di accesso remoto (solo in ingresso)** e **connessioni di routing con connessione a richiesta (in ingresso e in uscita)** .

    b. Scegliere **OK**.

20. Selezionare **WAN Miniport (L2TP)** e selezionare **Configura**. Verrà visualizzata la finestra di dialogo Configura dispositivo-WAN Miniport (L2TP).

    a. In numero **massimo di porte**immettere il numero di porte corrispondenti al numero massimo di connessioni VPN simultanee che si desidera supportare.

    b. Scegliere **OK**.

21. Selezionare **WAN Miniport (PPTP)** e selezionare **Configura**. Verrà visualizzata la finestra di dialogo Configura dispositivo-WAN Miniport (PPTP).

    a. In numero **massimo di porte**immettere il numero di porte corrispondenti al numero massimo di connessioni VPN simultanee che si desidera supportare.

    b. Scegliere **OK**.

22. Selezionare **miniport WAN (IKEv2)** e selezionare **Configura**. Verrà visualizzata la finestra di dialogo Configura dispositivo-WAN Miniport (IKEv2).

     a. In numero **massimo di porte**immettere il numero di porte corrispondenti al numero massimo di connessioni VPN simultanee che si desidera supportare.

     b. Scegliere **OK**.

23. Se richiesto, selezionare **Sì** per confermare il riavvio del server e selezionare **Chiudi** per riavviare il server.

## <a name="next-step"></a>Passaggio successivo

[Passaggio 4. Installare e configurare il server dei criteri di rete](vpn-deploy-nps.md): in questo passaggio si installa Server dei criteri di rete tramite Windows PowerShell o l'Server Manager aggiunta guidata ruoli e funzionalità. Viene inoltre configurato il server dei criteri di rete per gestire tutti i compiti di autenticazione, autorizzazione e accounting per le richieste di connessione ricevute dal server VPN.
