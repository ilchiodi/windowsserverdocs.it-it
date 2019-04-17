---
title: Configurare il server di accesso remoto per VPN Always On
description: RRAS è progettato per eseguire correttamente come un router e un server di accesso remoto. di conseguenza, supporta un'ampia gamma di funzionalità.
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067389"
---
# Passaggio 3. Configurare il server di accesso remoto per VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Precedente:** passaggio 2. Configurare l'infrastruttura di Server](vpn-deploy-server-infrastructure.md)<br>
& #187;  [ **Precedente:** passaggio 4. Installare e configurare Server dei criteri di rete (NPS)](vpn-deploy-nps.md)


RRAS è progettato per eseguire correttamente come un router e un server di accesso remoto in quanto supporta un'ampia gamma di funzionalità. Ai fini di questa distribuzione, richiedere solo un piccolo sottoinsieme di queste funzionalità: supporto per le connessioni VPN IKEv2 e il routing LAN.

IKEv2 è una connessione VPN descritto in Internet Engineering Task Force richiedere per commenti 7296 protocollo di tunneling. Il vantaggio principale di IKEv2 è che tollerata interruzioni nella connessione di rete sottostante. Ad esempio, se la connessione è temporaneamente persa o se un utente si sposta un computer client da una rete a un altro, IKEv2 Ripristina automaticamente la connessione VPN quando viene ripristinata la connessione di rete, ovvero tutto senza l'intervento dell'utente.

Configurare il server RRAS per supportare le connessioni IKEv2 durante la disattivazione protocolli inutilizzati, che consente di ridurre il footprint di sicurezza del server. Inoltre, configurare il server per assegnare gli indirizzi per i client VPN da un pool di indirizzo statico. È possibile assegnare facilmente indirizzi da un pool o un server DHCP. Tuttavia, usando un server DHCP aumenta la complessità della progettazione e offre vantaggi minime.


>[!Important]
>È importante:
>- Installare due schede di rete Ethernet in server fisici. Se stai installando il server VPN in una macchina virtuale, è necessario creare due commutatori di virtuali esterni, uno per ogni scheda di rete fisica; e quindi creare due schede di rete virtuale per la macchina virtuale, con ogni scheda di rete connessa a un commutatore virtuale.
>
>- Installare il server nella rete perimetrale tra il bordo e firewall interni, con una scheda di rete connesse alla rete perimetrale esterna e una scheda di rete connesse alla rete perimetrale interna.


>[!Warning]
>Prima di iniziare, assicurati di abilitare IPv6 sul server VPN. In caso contrario, non può essere stabilita una connessione e visualizza un messaggio di errore.

## Installare l'accesso remoto come Server VPN Gateway RAS

In questa procedura, si installa il ruolo di accesso remoto come server VPN di Gateway RAS singolo tenant. Per altre informazioni, vedi [Accesso remoto](../../../Remote-Access.md).


### Installa il ruolo di accesso remoto tramite Windows PowerShell

1. Apri Windows PowerShell come **amministratore**.

2. Digita il comando seguente e premi **INVIO**:

   `Install-WindowsFeature DirectAccess-VPN -IncludeManagementTools`

   Al termine dell'installazione, viene visualizzato il messaggio seguente in Windows PowerShell.
    
   | Operazioni riuscita | Riavvio necessario | Codice di uscita | Risultato delle funzionalità                             |
   |---------|----------------|-----------|--------------------------------------------|
   | True    | No             | Operazioni riuscita   | {Connection Manager Administration Kit RAS |
   ---

### Installa il ruolo di accesso remoto con Server Manager

È possibile utilizzare la procedura seguente per installare il ruolo di accesso remoto con Server Manager.

1.  Nel server VPN, in Server Manager, fai clic su **Gestisci** e fai clic su **Aggiungi ruoli e funzionalità**. <p>Verrà visualizzata l'aggiunta guidata ruoli e funzionalità.

2.  Nella prima iniziare a pagina, fai clic su**Avanti**.

3.  Nella pagina Selezione tipo di installazione, seleziona l'opzione di **installazione basato sui ruoli o basata su funzionalità** e fai clic su **Avanti**.

4.  Nella pagina Select destination server, seleziona l'opzione di **Selezionare un server dal pool di server** .

5.  Nel Pool di Server, seleziona il computer locale e fare clic su **Avanti**.

6.  Nella pagina ruoli server seleziona, **ruoli**, fai clic su **Accesso remoto**e quindi **Avanti**.

7.  Nella pagina selezionare le funzionalità, fai clic su **Avanti**.

8.  Nella pagina di accesso remoto, fai clic su **Avanti**.

9.  Nella pagina del servizio ruolo selezionare, **servizi ruolo**, fai clic su**DirectAccess e VPN (RAS)**.<p>Verrà visualizzata la finestra di dialogo **guidata Aggiungi ruoli e funzionalità** .

10. Nella finestra di dialogo funzionalità e Aggiungi ruoli del clic su **Aggiungi funzionalità** e fai clic su **Avanti**.

11. Nella pagina ruolo Server Web (IIS), fai clic su **Avanti**.

12. Nella pagina Seleziona ruolo Servizi, fai clic su **Avanti**.

13. Esaminare le scelte nella pagina Conferma installazione le selezioni e fai clic su **Installa**.

14. Una volta completata l'installazione, fai clic su **Close**.

## Configurare l'accesso remoto come Server VPN

In questa sezione, è possibile configurare l'accesso remoto VPN per consentire le connessioni VPN IKEv2, Nega le connessioni da altri protocolli VPN e assegnare un pool di indirizzi IP statici per il rilascio di indirizzi IP per la connessione client VPN autorizzati.

1.  Nel server VPN, in Server Manager, fai clic sul flag di **notifiche** .

2.  Nel menu di **attività** , fare clic su **Apri la procedura guidata introduttiva**.<p>Si apre la procedura guidata Configura accesso remoto. 

    >[!NOTE] 
    >La procedura guidata Configura accesso remoto possa Apri dietro Server Manager. Se si ritiene che impiega troppo tempo per aprire la procedura guidata, spostare o ridurre al minimo di Server Manager per scoprire se la procedura guidata è dietro di esso. In caso contrario, attendere la procedura guidata inizializzare.

3.  Fai clic su **distribuire solo VPN**.<p>Si apre il Routing e Remote Access Microsoft Management Console (MMC).

4.  Il pulsante destro del server VPN e fai clic su **Configura e abilita Routing e accesso remoto**.<p>Verrà visualizzata la configurazione guidata Server accesso remoto e di Routing.

5.  Nella schermata iniziale per il Routing e la configurazione guidata Server accesso remoto, fare clic su **Avanti**.

6.  Nella **configurazione**, fai clic su **Configurazione personalizzata**e quindi fai clic su **Avanti**.

7.  Nella **Configurazione personalizzata**, fai clic su **accesso VPN**e quindi fai clic su **Avanti**.<p>Completamento si apre il Routing e configurazione guidata Server accesso remoto.

8.  Fai clic su **Fine** per chiudere la procedura guidata e fai clic su **OK** per chiudere la finestra di dialogo Routing e accesso remoto.

9.  Scegliere di **avviare il servizio** per avviare l'accesso remoto.

10. In MMC di accesso remoto, destro del mouse sul server VPN e fare clic su **proprietà**.

11. Nella proprietà, fare clic sulla scheda **sicurezza** ed eseguire:

    a. Fai clic su **provider di autenticazione** e fai clic su **Autenticazione RADIUS**.
    
    b. Fai clic su **Configura**.<p>Viene visualizzata la finestra di dialogo autenticazione RADIUS.
    
    c. Fai clic su **Aggiungi**.<p>Si apre la finestra di dialogo Aggiungi Server RADIUS.
    
    d. **Nome del Server**, digita il nome di dominio completo (FQDN) del server dei criteri di rete nella rete aziendale o dell'organizzazione.<p>Ad esempio, se il nome NetBIOS del server dei criteri di rete è NPS1 e il nome di dominio è corp.contoso.com, digitare **NPS1.corp.contoso.com**.
    
    e. Nel **segreto condiviso**, fai clic su **Modifica**.<p>Si apre la finestra di dialogo Modifica segreto.
    
    f. Nel **nuovo segreto**, digitare una stringa di testo.
    
    g. Nella finestra di **Conferma nuovo segreto**, digita la stessa stringa di testo e fai clic su **OK**.

    >[!IMPORTANT] 
    >Salva questa stringa di testo. Quando si configura il Server dei criteri di rete nella rete aziendale o dell'organizzazione, aggiungerai questo Server VPN come Client RADIUS. Durante la configurazione, utilizzerai questo stesso segreto condiviso in modo che i criteri di rete e server VPN possono comunicare.

12. In **Aggiunta Server RADIUS**, esaminare le impostazioni predefinite per:

    - **Timeout**
    
    - **Punteggio iniziale**
    
    - **Port**

13. Se necessario, cambiare i valori per associare i requisiti per il tuo ambiente e fai clic su **OK**.<p>NAS è un dispositivo che fornisce un certo livello di accesso a una rete di grandi dimensioni. Utilizza un'infrastruttura RADIUS NAS è anche un client RADIUS, inviare richieste di connessione e i messaggi di accounting a un server RADIUS per l'autenticazione, autorizzazione e l'accounting.

14. Esaminare l'impostazione per **provider di Accounting**:

    | Se vuoi che il...  | Allora...             |
    |---------------------|-------------------|
    | Attività di accesso remoto registrato nel server di accesso remoto | Assicurati che sia selezionato **Accounting di Windows** .      |
    | Criteri di rete per eseguire i servizi di contabilità per la VPN   | Modificare i **provider di Accounting** di **Accounting RADIUS** e quindi Configura i criteri di rete come provider di accounting. |
    ---

15. Fare clic sulla scheda **IPv4** ed eseguire:

    a. Fai clic sul **pool indirizzo statico**.
    
    b. Fai clic su **Aggiungi** per configurare un pool di indirizzi IP.<p>Il pool di indirizzo statico deve contenere gli indirizzi della rete perimetrale interni. Questi indirizzi sono sulla connessione di rete interna nel server VPN, non alla rete azienda.
    
    c. **Indirizzo IP iniziale**, digita l'indirizzo IP iniziale nell'intervallo di cui che si desidera assegnare ai client VPN.
    
    d. **Indirizzo IP finale**, digita l'indirizzo IP finale nell'intervallo di cui che si desidera assegnare ai client VPN o in **numero di indirizzi**, digita il numero di quello che vuoi rendere disponibili. Se stai usando DHCP per la subnet, assicurati che puoi configurare una corrispondenti esclusione di indirizzi dei server DHCP.
    
    e. (Facoltativo) Se si utilizza DHCP, fai clic sulla **scheda**e nell'elenco dei risultati, fare clic sulla scheda Ethernet connessa alla rete perimetrale interni.

16. (Facoltativo) *Se vuoi configurare l'accesso condizionale per la connettività VPN*, dall'elenco a discesa di **certificati** , sotto **Il Binding certificato SSL**, seleziona l'autenticazione del server VPN.

17. (Facoltativo) *Se vuoi configurare l'accesso condizionale per la connettività VPN*, in di MMC NPS espandere **Policies\\Network criteri** ed eseguire: 

    a. Criteri di rete **le connessioni a Microsoft Routing e accesso remoto** the a destra e seleziona **proprietà**.
    
    b. Seleziona il concedere l'accesso **. Concedere l'accesso se la richiesta di connessione corrispondente a questo criterio** opzione.
    
    c. Selezionare **Il Server di accesso remoto (VPN-connessione remota)** tipo di server di accesso di rete, dal menu a discesa.

3.  Nel Routing MMC e accesso remoto, fai **porte** e quindi fai clic su **proprietà**. <p>Viene visualizzata la finestra di dialogo proprietà delle porte.

4.  Fai clic su **WAN Miniport SSTP ()** e fare clic su **Configura**. Il dispositivo Configure - apre la finestra di dialogo WAN Miniport SSTP ().

    a. Deseleziona le caselle di controllo **connessioni di accesso remoto (solo in ingresso)** e **routing connessioni a richiesta (in entrata e in uscita)** .
    
    b. Scegli **OK**.

5.  Fai clic su **WAN Miniport (L2TP)** e fare clic su **Configura**. Il dispositivo Configure - apre la finestra di dialogo WAN Miniport (L2TP).

    a. **Numero massimo di porte**, digita il numero di porte in modo che corrisponda il numero massimo di connessioni VPN simultanee che vuoi supportare.
    
    b. Scegli **OK**.

6.  Fai clic su **WAN Miniport (PPTP)** e fare clic su **Configura**. Il dispositivo Configure - apre la finestra di dialogo WAN Miniport (PPTP).

    a. **Numero massimo di porte**, digita il numero di porte in modo che corrisponda il numero massimo di connessioni VPN simultanee che vuoi supportare.
    
    b. Scegli **OK**.
    
7. Fai clic su **WAN Miniport (IKEv2)** e fare clic su **Configura**. Il dispositivo Configure - apre la finestra di dialogo Miniport WAN (IKEv2).

    a. **Numero massimo di porte**, digita il numero di porte in modo che corrisponda il numero massimo di connessioni VPN simultanee che vuoi supportare.
    
    b. Scegli **OK**.

7.  Se richiesto, fai clic su **Sì** per confermare il riavvio del server e fare clic su **Close** per riavviare il server.

## Passaggio successivo
[Passaggio 4. Installare e configurare Server dei criteri di rete (NPS)](vpn-deploy-nps.md): In questo passaggio, installare Server dei criteri di rete (NPS) usando sia Windows PowerShell o il Server Manager Aggiunta guidata ruoli e funzionalità. Anche configurare dei criteri di rete per gestire l'autenticazione, autorizzazione e compiti contabilità per la connessione tutte le richieste che riceve dal server VPN.





---
