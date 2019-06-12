---
title: Configurare il server di accesso remoto per VPN Always On
description: RRAS è progettato per essere eseguita anche da un router sia un server di accesso remoto; Pertanto, supporta un'ampia gamma di funzionalità.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.reviewer: deverette
ms.openlocfilehash: 3920f7f075f4742a62577ade809cc0494b05bf1f
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749434"
---
# <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>Passaggio 3. Configurare il server di accesso remoto per VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 2. Configurare l'infrastruttura di Server](vpn-deploy-server-infrastructure.md)
- [**Precedente:** Passaggio 4. Installare e configurare il Server dei criteri di rete (NPS)](vpn-deploy-nps.md)

RRAS è progettato per eseguire anche come un router sia un server di accesso remoto, perché supporta un'ampia gamma di funzionalità. Ai fini di questa distribuzione, è necessario solo un piccolo subset di queste funzionalità: supporto per le connessioni VPN IKEv2 e routing LAN.

IKEv2 è una protocollo descritto in Internet Engineering Task Force richiesta per i commenti 7296 di tunneling VPN. Il vantaggio principale di IKEv2 è che tollera interruzioni della connessione di rete sottostante. Ad esempio, se la connessione viene temporaneamente persa o se un utente si sposta un computer client da una rete a un altro, IKEv2 Ripristina automaticamente la connessione VPN quando viene ristabilita la connessione di rete, ovvero senza l'intervento dell'utente.

Configurare il server RRAS per supportare le connessioni IKEv2 durante la disabilitazione protocolli inutilizzati, riducendo così il footprint di sicurezza del server. Inoltre, configurare il server per assegnare gli indirizzi ai client VPN da un pool di indirizzi statici. È possibile assegnare indirizzi poturo da un pool o un server DHCP. Tuttavia, tramite un server DHCP aggiunge complessità alla progettazione e offre vantaggi minimi.

>[!IMPORTANT]
>È importante:
>- Installare due schede di rete Ethernet nel server fisico. Se si installa il server VPN in una macchina virtuale, è necessario creare due opzioni di virtuale esterni, una per ogni scheda di rete fisica; e quindi creare due schede di rete virtuale per la macchina virtuale, con ogni scheda di rete connessa a un commutatore virtuale.
>
>- Installare il server nella rete perimetrale tra il bordo e il firewall interno, con una scheda di rete connesse alla rete perimetrale esterni e una scheda di rete connesse alla rete perimetrale interna.

>[!WARNING]
>Prima di iniziare, assicurarsi di abilitare IPv6 del server VPN. In caso contrario, non è possibile stabilire una connessione e viene visualizzato un messaggio di errore.

## <a name="install-remote-access-as-a-ras-gateway-vpn-server"></a>Installare accesso remoto come Server VPN Gateway RAS

In questa procedura installare il ruolo Accesso remoto come server VPN Gateway RAS single-tenant. Per altre informazioni, vedi [Accesso remoto](../../../Remote-Access.md).

### <a name="install-the-remote-access-role-by-using-windows-powershell"></a>Installare il ruolo Accesso remoto tramite Windows PowerShell

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

### <a name="install-the-remote-access-role-by-using-server-manager"></a>Installare il ruolo Accesso remoto con Server Manager

È possibile utilizzare la procedura seguente per installare il ruolo Accesso remoto con Server Manager.

1. Nel server VPN, in Server Manager, selezionare **Manage** e selezionare **Aggiungi ruoli e funzionalità**.
   
   Viene avviata l'Aggiunta guidata ruoli e funzionalità.

2. Nella prima pagina di avvio, selezionare **successivo**.

3. Nella pagina Selezione tipo di installazione, selezionare la **installazione basata su ruoli o basata su funzionalità** opzione e selezionare **successivo**.

4. Nella pagina del server di destinazione, selezionare la **selezionare un server dal pool di server** opzione.

5. Nel Pool di Server, selezionare il computer locale e selezionare **successivo**.

6. In Selezione ruoli server nella pagina **ruoli**, selezionare **accesso remoto**, quindi **Next**.

7. Nella pagina Selezione funzionalità selezionare **successivo**.

8. Nella pagina di accesso remoto, selezionare **successivo**.

9.  Nella pagina Selezione ruolo del servizio, in **servizi ruolo**, selezionare **DirectAccess e VPN (RAS)** .

   Il **Aggiunta guidata ruoli e funzionalità** verrà visualizzata la finestra di dialogo.

11. Nella finestra di dialogo Aggiungi ruoli e funzionalità, selezionare **Aggiungi funzionalità** quindi selezionare **successivo**.

12. Nella pagina del ruolo Server Web (IIS), selezionare **successivo**.

13. La pagina Selezione servizi ruolo, scegliere **successivo**.

14. Nella pagina Conferma selezioni per l'installazione, rivedere le scelte effettuate, quindi selezionare **installare**.

15. Una volta completata l'installazione, selezionare **Chiudi**.

## <a name="configure-remote-access-as-a-vpn-server"></a>Configurare l'accesso remoto come Server VPN

In questa sezione, è possibile configurare accesso remoto VPN per consentire le connessioni VPN IKEv2, negare le connessioni da altri protocolli VPN e assegnare un pool di indirizzi IP statici per il rilascio degli indirizzi IP per la connessione client VPN autorizzati.

1. Nel server VPN, in Server Manager, selezionare la **notifiche** flag.

2. Nel **attività** dal menu **aprire attività iniziali guidate**

   Verrà visualizzata la procedura guidata Configura accesso remoto.

   >[!NOTE]
   >La procedura guidata Configura accesso remoto potrebbe aprire dietro a Server Manager. Se si ritiene che la procedura guidata sta richiedendo troppo tempo per aprire, spostare o ridurre al minimo di Server Manager per determinare se la procedura guidata è dietro di essa. In caso contrario, attendere che la procedura guidata per l'inizializzazione.

3. Selezionare **distribuire solo VPN**.

    Si apre il Routing e Remote Access Microsoft Management Console (MMC).

4. Fare doppio clic il server VPN, quindi selezionare **Configura e abilita Routing e accesso remoto**.

   Verrà visualizzata la configurazione guidata Server di accesso remoto e Routing.

5. Nella pagina iniziale per il Routing e la configurazione guidata Server di accesso remoto, selezionare **successivo**.

6. Nella **Configuration**, selezionare **configurazione personalizzata**, quindi selezionare **Avanti**.

7. Nella **configurazione personalizzata**, selezionare **accesso VPN**, quindi selezionare **Avanti**.

   Completamento viene visualizzata la configurazione guidata Server di accesso remoto e Routing.

8. Selezionare **Finish** per chiudere la procedura guidata, quindi selezionare **OK** per chiudere la finestra di dialogo Routing e accesso remoto.

9. Selezionare **avviare il servizio** per avviare l'accesso remoto.

10. In MMC accesso remoto, fare clic sul server VPN, quindi selezionare **proprietà**.

11. Nella finestra Proprietà, selezionare la **sicurezza** scheda ed eseguire:

    a. Selezionare **provider di autenticazione** e selezionare **autenticazione RADIUS**.

    b. Selezionare **configurare**.

       Verrà visualizzata la finestra di dialogo autenticazione RADIUS.

    c. Selezionare **Aggiungi**.

       Verrà visualizzata la finestra di dialogo Aggiungi Server RADIUS.

    d. Nelle **nome Server**, immettere il nome di dominio completo (FQDN) del server dei criteri di rete nella rete aziendale o dell'organizzazione.
    
       Ad esempio, se il nome NetBIOS del server dei criteri di rete è NPS1 e il nome di dominio è corp.contoso.com, immettere **NPS1.corp.contoso.com**.

    e. Nelle **segreto condiviso**, selezionare **modifica**.

       Verrà visualizzata la finestra di dialogo Modifica segreto.

    f. Nelle **nuovo master secret**, immettere una stringa di testo.

    g. Nelle **Conferma nuovo master secret**, immettere la stessa stringa di testo, quindi selezionare **OK**.

    >[!IMPORTANT]
    >Salvare la stringa di testo. Quando si configura il Server dei criteri di rete nella rete aziendale o dell'organizzazione, si aggiungerà il Server VPN come Client RADIUS. Durante la configurazione, si userà questo stesso segreto condiviso in modo che i criteri di rete e server VPN possano comunicare.

12. Nelle **Aggiungi Server RADIUS**, rivedere le impostazioni predefinite per:

    - **Time-out**

    - **Punteggio di iniziale**

    - **Porta**

13. Se necessario, modificare i valori in base ai requisiti per l'ambiente e selezionare **OK**.

    Un server NAS è un dispositivo che fornisce un livello di accesso a una rete più estesa. Un server NAS usando un'infrastruttura RADIUS è anche un client RADIUS, l'invio di messaggi di accounting e le richieste di connessione a un server RADIUS per autenticazione, autorizzazione e accounting.

14. Esaminare le impostazioni per **provider di Accounting**:

    |                    Se si desidera che il...                     |                                                     Allora...                                                      |
    |-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
    | Attività di accesso remoto registrato nel server di accesso remoto |                               Verificare che l'opzione **Accounting Windows** sia selezionata.                               |
    |        Criteri di rete per eseguire i servizi di contabilità per VPN         | Change **provider di Accounting** al **Accounting RADIUS** e quindi configurare i criteri di rete come provider di accounting. |

15. Selezionare il **IPv4** scheda ed eseguire:

    a. Selezionare **pool di indirizzi statici**.

    b. Selezionare **Add** per configurare un pool di indirizzi IP.

       Il pool di indirizzi statici può contenere indirizzi della rete perimetrale interna. Questi indirizzi sono per la connessione di rete interna nel server VPN, non alla rete aziendale.

    c. Nelle **indirizzo IP iniziale**, immettere l'indirizzo IP iniziale dell'intervallo da assegnare ai client VPN.

    d. Nelle **indirizzo IP finale**, immettere l'indirizzo IP finale nell'intervallo da assegnare ai client VPN o nella **numero di indirizzi**, immettere il numero dell'indirizzo a cui si desidera rendere disponibili. Se si usa DHCP per la subnet, assicurarsi di configurare un'esclusione di indirizzi corrispondente nei server DHCP.

    e. (Facoltativo) Se si usa DHCP, selezionare **Adapter**e nell'elenco dei risultati, selezionare la scheda Ethernet connessa alla rete perimetrale interna.

16. (Facoltativo) *Se si configura l'accesso condizionale per la connettività VPN*, dalle **certificato** elenco a discesa elencare, sotto **Binding a certificati SSL**, selezionare il server VPN autenticazione.

17. (Facoltativo) *Se si configura l'accesso condizionale per la connettività VPN*, in NPS MMC, espandere **criteri\\i criteri di rete** ed eseguire: 

    a. Il diritto **le connessioni al servizio Routing e accesso remoto** dei criteri di rete e selezionare **proprietà**.

    b. Selezionare il **concedere l'accesso. Concedere l'accesso se la richiesta di connessione corrisponde a questo criterio** opzione.

    c. In tipo di server di accesso di rete, selezionare **Server di accesso remoto (VPN-accesso remoto)** dall'elenco a discesa.

18. In MMC di Routing e Remote Access, fare doppio clic **porte** e quindi selezionare **proprietà**. 
    
    Verrà aperta la finestra di dialogo delle proprietà delle porte.

19. Selezionare **Miniport WAN (SSTP)** e selezionare **configura**. Verrà visualizzata la finestra di dialogo Miniport WAN (SSTP) Configura dispositivo.

    a. Cancella il **le connessioni di accesso remoto (solo in ingresso)** e **connessioni di routing a richiesta (in ingresso e in uscita)** caselle di controllo.

    b. Scegliere **OK**.

20. Selezionare **Miniport rete WAN (L2TP)** e selezionare **configura**. Configura dispositivo - finestra di dialogo Miniport WAN (L2TP).

    a. Nelle **numero massimo di porte**, immettere il numero di porte in modo che corrisponda il numero massimo di connessioni VPN simultanee che si desidera supportare.

    b. Scegliere **OK**.

21. Selezionare **WAN Miniport (PPTP)** e selezionare **configura**. Verrà visualizzata la finestra di dialogo WAN Miniport (PPTP) Configura dispositivo.

    a. Nelle **numero massimo di porte**, immettere il numero di porte in modo che corrisponda il numero massimo di connessioni VPN simultanee che si desidera supportare.

    b. Scegliere **OK**.

22. Selezionare **Miniport rete WAN (IKEv2)** e selezionare **configura**. Configura dispositivo - finestra di dialogo Miniport WAN (IKEv2).

     a. Nelle **numero massimo di porte**, immettere il numero di porte in modo che corrisponda il numero massimo di connessioni VPN simultanee che si desidera supportare.

     b. Scegliere **OK**.

23. Se richiesto, selezionare **Yes** per confermare il riavvio di server e selezionare **Chiudi** per riavviare il server.

## <a name="next-step"></a>Passaggio successivo

[Passaggio 4. Installare e configurare il Server dei criteri di rete (NPS)](vpn-deploy-nps.md): In questo passaggio per installare Server dei criteri di rete (NPS), usando Windows PowerShell o il Server Manager Aggiungi ruoli e funzionalità guidata. È inoltre configurare criteri di rete per gestire l'autenticazione, autorizzazione e compiti di contabilità per la connessione tutte le richieste ricevute dal server VPN.
