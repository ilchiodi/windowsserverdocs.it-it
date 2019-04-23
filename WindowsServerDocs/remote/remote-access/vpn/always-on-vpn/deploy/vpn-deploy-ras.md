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
ms.openlocfilehash: a338ddfec1ed5cd0e9198f64dc4952eb591cdc1b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829672"
---
# <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>Passaggio 3. Configurare il server di accesso remoto per VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Precedente:** Passaggio 2. Configurare l'infrastruttura di Server](vpn-deploy-server-infrastructure.md)<br>
&#187;  [**Precedente:** Passaggio 4. Installare e configurare il Server dei criteri di rete (NPS)](vpn-deploy-nps.md)


RRAS è progettato per eseguire anche come un router sia un server di accesso remoto, perché supporta un'ampia gamma di funzionalità. Ai fini di questa distribuzione, è necessario solo un piccolo subset di queste funzionalità: supporto per le connessioni VPN IKEv2 e routing LAN.

IKEv2 è una protocollo descritto in Internet Engineering Task Force richiesta per i commenti 7296 di tunneling VPN. Il vantaggio principale di IKEv2 è che tollera interruzioni della connessione di rete sottostante. Ad esempio, se la connessione viene temporaneamente persa o se un utente si sposta un computer client da una rete a un altro, IKEv2 Ripristina automaticamente la connessione VPN quando viene ristabilita la connessione di rete, ovvero senza l'intervento dell'utente.

Configurare il server RRAS per supportare le connessioni IKEv2 durante la disabilitazione protocolli inutilizzati, riducendo così il footprint di sicurezza del server. Inoltre, configurare il server per assegnare gli indirizzi ai client VPN da un pool di indirizzi statici. È possibile assegnare indirizzi poturo da un pool o un server DHCP. Tuttavia, tramite un server DHCP aggiunge complessità alla progettazione e offre vantaggi minimi.


>[!Important]
>È importante:
>- Installare due schede di rete Ethernet nel server fisico. Se si installa il server VPN in una macchina virtuale, è necessario creare due opzioni di virtuale esterni, una per ogni scheda di rete fisica; e quindi creare due schede di rete virtuale per la macchina virtuale, con ogni scheda di rete connessa a un commutatore virtuale.
>
>- Installare il server nella rete perimetrale tra il bordo e il firewall interno, con una scheda di rete connesse alla rete perimetrale esterni e una scheda di rete connesse alla rete perimetrale interna.


>[!Warning]
>Prima di iniziare, assicurarsi di abilitare IPv6 del server VPN. In caso contrario, non è possibile stabilire una connessione e viene visualizzato un messaggio di errore.

## <a name="install-remote-access-as-a-ras-gateway-vpn-server"></a>Installare accesso remoto come Server VPN Gateway RAS

In questa procedura installare il ruolo Accesso remoto come server VPN Gateway RAS single-tenant. Per altre informazioni, vedi [Accesso remoto](../../../Remote-Access.md).


### <a name="install-the-remote-access-role-by-using-windows-powershell"></a>Installare il ruolo Accesso remoto tramite Windows PowerShell

1. Aprire Windows PowerShell come **amministratore**.

2. Digitare il seguente comando e premere **invio**:

   `Install-WindowsFeature DirectAccess-VPN -IncludeManagementTools`

   Al termine dell'installazione, viene visualizzato il messaggio seguente in Windows PowerShell.
    
   | Riuscito | Riavvio necessario | Codice di chiusura | Risultato di funzionalità                             |
   |---------|----------------|-----------|--------------------------------------------|
   | True    | No             | Riuscito   | {RAS Connection Manager Administration Kit |
   ---

### <a name="install-the-remote-access-role-by-using-server-manager"></a>Installare il ruolo Accesso remoto con Server Manager

È possibile utilizzare la procedura seguente per installare il ruolo Accesso remoto con Server Manager.

1.  Nel server VPN, in Server Manager fare clic su **Manage** e fare clic su **Aggiungi ruoli e funzionalità**. <p>Viene avviata l'Aggiunta guidata ruoli e funzionalità.

2.  Nella prima pagina di avvio, fare clic su **successivo**.

3.  Nella pagina Selezione tipo di installazione, selezionare la **installazione basata su ruoli o basata su funzionalità** opzione e fare clic su **successivo**.

4.  Nella pagina del server di destinazione, selezionare la **selezionare un server dal pool di server** opzione.

5.  Nel Pool di Server, selezionare il computer locale e fare clic su **successivo**.

6.  In Selezione ruoli server nella pagina **ruoli**, fare clic su **accesso remoto**e quindi **Next**.

7.  Nella pagina Selezione funzionalità fare clic su **Avanti**.

8.  Nella pagina di accesso remoto, fare clic su **successivo**.

9.  Nella pagina Selezione ruolo del servizio, in **servizi ruolo**, fare clic su **DirectAccess e VPN (RAS)**.<p>Il **Aggiunta guidata ruoli e funzionalità** verrà visualizzata la finestra di dialogo.

10. Nella finestra di dialogo Aggiungi ruoli e funzionalità, fare clic su **Aggiungi funzionalità** e fare clic su **successivo**.

11. Nella pagina ruolo Server Web (IIS), fare clic su **successivo**.

12. La pagina Selezione servizi ruolo, scegliere **successivo**.

13. Nella pagina Conferma selezioni per l'installazione, rivedere le scelte effettuate e fare clic su **installare**.

14. Al termine dell'installazione fare clic su **Chiudi**.

## <a name="configure-remote-access-as-a-vpn-server"></a>Configurare l'accesso remoto come Server VPN

In questa sezione, è possibile configurare accesso remoto VPN per consentire le connessioni VPN IKEv2, negare le connessioni da altri protocolli VPN e assegnare un pool di indirizzi IP statici per il rilascio degli indirizzi IP per la connessione client VPN autorizzati.

1.  Nel server VPN, in Server Manager fare clic sui **notifiche** flag.

2.  Nel **attività** menu, fare clic su **aprire la procedura guidata introduttiva**.<p>Verrà visualizzata la procedura guidata Configura accesso remoto. 

    >[!NOTE] 
    >La procedura guidata Configura accesso remoto potrebbe aprire dietro a Server Manager. Se si ritiene che la procedura guidata sta richiedendo troppo tempo per aprire, spostare o ridurre al minimo di Server Manager per determinare se la procedura guidata è dietro di essa. In caso contrario, attendere che la procedura guidata per l'inizializzazione.

3.  Fare clic su **distribuire solo VPN**.<p>Si apre il Routing e Remote Access Microsoft Management Console (MMC).

4.  Visualizzare il server VPN, quindi scegliere **Configura e abilita Routing e accesso remoto**.<p>Verrà visualizzata la configurazione guidata Server di accesso remoto e Routing.

5.  Nella pagina iniziale per il Routing e la configurazione guidata Server di accesso remoto, fare clic su **successivo**.

6.  Nella **Configuration**, fare clic su **configurazione personalizzata**, quindi fare clic su **Avanti**.

7.  Nelle **configurazione personalizzata**, fare clic su **accesso VPN**, quindi fare clic su **Avanti**.<p>Completamento viene visualizzata la configurazione guidata Server di accesso remoto e Routing.

8.  Fare clic su **Finish** per chiudere la procedura guidata, fare clic su **OK** per chiudere la finestra di dialogo Routing e accesso remoto.

9.  Fare clic su **avviare il servizio** per avviare l'accesso remoto.

10. In MMC accesso remoto, fare doppio clic su server VPN e scegliere **proprietà**.

11. Nelle proprietà, fare clic sui **sicurezza** scheda ed eseguire:

    a. Fare clic su **provider di autenticazione** e fare clic su **autenticazione RADIUS**.
    
    b. Fare clic su **configurare**.<p>Verrà visualizzata la finestra di dialogo autenticazione RADIUS.
    
    c. Fai clic su **Aggiungi**.<p>Verrà visualizzata la finestra di dialogo Aggiungi Server RADIUS.
    
    d. Nelle **nome Server**, digitare il nome di dominio completo (FQDN) del server dei criteri di rete nella rete aziendale o dell'organizzazione.<p>Ad esempio, se il nome NetBIOS del server dei criteri di rete è NPS1 e il nome di dominio è corp.contoso.com, digitare **NPS1.corp.contoso.com**.
    
    e. Nelle **segreto condiviso**, fare clic su **modifica**.<p>Verrà visualizzata la finestra di dialogo Modifica segreto.
    
    f. Nelle **nuovo master secret**, digitare una stringa di testo.
    
    g. Nelle **Conferma nuovo master secret**, digitare la stessa stringa di testo e fare clic su **OK**.

    >[!IMPORTANT] 
    >Salvare la stringa di testo. Quando si configura il Server dei criteri di rete nella rete aziendale o dell'organizzazione, si aggiungerà il Server VPN come Client RADIUS. Durante la configurazione, si userà questo stesso segreto condiviso in modo che i criteri di rete e server VPN possano comunicare.

12. Nelle **Aggiungi Server RADIUS**, rivedere le impostazioni predefinite per:

    - **Time-out**
    
    - **Punteggio di iniziale**
    
    - **Porta**

13. Se necessario, modificare i valori in base ai requisiti per l'ambiente e fare clic su **OK**.<p>Un server NAS è un dispositivo che fornisce un livello di accesso a una rete più estesa. Un server NAS usando un'infrastruttura RADIUS è anche un client RADIUS, l'invio di messaggi di accounting e le richieste di connessione a un server RADIUS per autenticazione, autorizzazione e accounting.

14. Esaminare le impostazioni per **provider di Accounting**:

    | Se si desidera che il...  | Allora...             |
    |---------------------|-------------------|
    | Attività di accesso remoto registrato nel server di accesso remoto | Verificare che l'opzione **Accounting Windows** sia selezionata.      |
    | Criteri di rete per eseguire i servizi di contabilità per VPN   | Change **provider di Accounting** al **Accounting RADIUS** e quindi configurare i criteri di rete come provider di accounting. |
    ---

15. Scegliere il **IPv4** scheda ed eseguire:

    a. Fare clic su **pool di indirizzi statici**.
    
    b. Fare clic su **Add** per configurare un pool di indirizzi IP.<p>Il pool di indirizzi statici può contenere indirizzi della rete perimetrale interna. Questi indirizzi sono per la connessione di rete interna nel server VPN, non alla rete aziendale.
    
    c. Nelle **indirizzo IP iniziale**, digitare l'indirizzo IP iniziale dell'intervallo da assegnare ai client VPN.
    
    d. Nelle **indirizzo IP finale**, digitare l'indirizzo IP finale nell'intervallo da assegnare ai client VPN o nella **numero di indirizzi**, digitare il numero dell'indirizzo a cui si desidera rendere disponibili. Se si usa DHCP per la subnet, assicurarsi di configurare un'esclusione di indirizzi corrispondente nei server DHCP.
    
    e. (Facoltativo) Se si usa DHCP, fare clic su **Adapter**e nell'elenco dei risultati, fare clic sulla scheda Ethernet connessa alla rete perimetrale interna.

16. (Facoltativo) *Se si configura l'accesso condizionale per la connettività VPN*, dalle **certificato** elenco a discesa elencare, sotto **Binding a certificati SSL**, selezionare il server VPN autenticazione.

17. (Facoltativo) *Se si configura l'accesso condizionale per la connettività VPN*, in NPS MMC, espandere **criteri\\i criteri di rete** ed eseguire: 

    a. Il diritto **le connessioni al servizio Routing e accesso remoto** dei criteri di rete e selezionare **proprietà**.
    
    b. Selezionare il **concedere l'accesso. Concedere l'accesso se la richiesta di connessione corrisponde a questo criterio** opzione.
    
    c. In tipo di server di accesso di rete, selezionare **Server di accesso remoto (VPN-accesso remoto)** dall'elenco a discesa.

3.  In MMC di Routing e Remote Access, fare doppio clic **porte** e quindi fare clic su **proprietà**. <p>Verrà aperta la finestra di dialogo delle proprietà delle porte.

4.  Fare clic su **Miniport WAN (SSTP)** e fare clic su **configura**. Verrà visualizzata la finestra di dialogo Miniport WAN (SSTP) Configura dispositivo.

    a. Cancella il **le connessioni di accesso remoto (solo in ingresso)** e **connessioni di routing a richiesta (in ingresso e in uscita)** caselle di controllo.
    
    b. Fare clic su **OK**.

5.  Fare clic su **Miniport rete WAN (L2TP)** e fare clic su **configura**. Configura dispositivo - finestra di dialogo Miniport WAN (L2TP).

    a. Nelle **numero massimo di porte**, digitare il numero di porte in modo che corrisponda il numero massimo di connessioni VPN simultanee che si desidera supportare.
    
    b. Fare clic su **OK**.

6.  Fare clic su **WAN Miniport (PPTP)** e fare clic su **configura**. Verrà visualizzata la finestra di dialogo WAN Miniport (PPTP) Configura dispositivo.

    a. Nelle **numero massimo di porte**, digitare il numero di porte in modo che corrisponda il numero massimo di connessioni VPN simultanee che si desidera supportare.
    
    b. Fare clic su **OK**.
    
7. Fare clic su **Miniport rete WAN (IKEv2)** e fare clic su **configura**. Configura dispositivo - finestra di dialogo Miniport WAN (IKEv2).

    a. Nelle **numero massimo di porte**, digitare il numero di porte in modo che corrisponda il numero massimo di connessioni VPN simultanee che si desidera supportare.
    
    b. Fare clic su **OK**.

7.  Se richiesto, fare clic su **Yes** per confermare il riavvio del server e scegliendo **Chiudi** per riavviare il server.

## <a name="next-step"></a>Passaggio successivo
[Passaggio 4. Installare e configurare il Server dei criteri di rete (NPS)](vpn-deploy-nps.md): In questo passaggio per installare Server dei criteri di rete (NPS), usando Windows PowerShell o il Server Manager Aggiungi ruoli e funzionalità guidata. È inoltre configurare criteri di rete per gestire l'autenticazione, autorizzazione e compiti di contabilità per la connessione tutte le richieste ricevute dal server VPN.





---
