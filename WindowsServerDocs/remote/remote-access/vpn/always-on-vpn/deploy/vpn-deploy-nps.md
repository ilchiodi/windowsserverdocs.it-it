---
title: Installare e configurare il Server dei criteri di rete
description: L'elaborazione del server dei criteri di rete per le richieste di connessione inviate dal server VPN verifica che l'utente disponga dell'autorizzazione per la connessione, dell'identità dell'utente e registra gli aspetti della richiesta di connessione scelti durante la configurazione dell'accounting RADIUS nel server dei criteri di rete.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 553f3327e6252d2b03744b2e0fc88f340701f3a9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871329"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>Passaggio 4. Installare e configurare il server dei criteri di rete

> Si applica a Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Prossimo** Passaggio 3. Configurare il server di accesso remoto per VPN Always On](vpn-deploy-ras.md)
- [**Prossimo** Passaggio 5. Configurare le impostazioni di firewall e DNS](vpn-deploy-dns-firewall.md)

In questo passaggio verrà installato Server dei criteri di rete per l'elaborazione delle richieste di connessione inviate dal server VPN:

- Eseguire l'autorizzazione per verificare che l'utente disponga dell'autorizzazione per la connessione.
- Esecuzione dell'autenticazione per verificare l'identità dell'utente.
- Esecuzione dell'accounting per registrare gli aspetti della richiesta di connessione selezionata durante la configurazione dell'accounting RADIUS in NPS.

I passaggi descritti in questa sezione consentono di completare gli elementi seguenti:

1. Nel computer o nella macchina virtuale che ha pianificato il server NPS e installato nell'organizzazione o nella rete aziendale, è possibile installare NPS.

   >[!TIP]
   >Se nella rete sono già presenti uno o più server dei criteri di rete, non è necessario eseguire l'installazione del server dei criteri di rete, ma è possibile utilizzare questo argomento per aggiornare la configurazione di un server dei criteri di rete esistente.

> [!NOTE]
> Non è possibile installare il servizio Server dei criteri di rete in Windows Server Core.

2. Nel server dei criteri di rete aziendale e aziendale è possibile configurare NPS per l'esecuzione come server RADIUS che elabora le richieste di connessione ricevute dal server VPN.

## <a name="install-network-policy-server"></a>Installare il server dei criteri di rete

In questa procedura viene installato NPS utilizzando Windows PowerShell o Server Manager l'aggiunta guidata ruoli e funzionalità. Server dei criteri di rete è un servizio ruolo del ruolo server Servizi di accesso e criteri di rete.

>[!TIP]
>Per impostazione predefinita, Server dei criteri di rete rimane in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 di tutte le schede di rete installate. Quando si installa NPS e si Abilita Windows Firewall con sicurezza avanzata, le eccezioni del firewall per queste porte vengono create automaticamente per il traffico IPv4 e IPv6. Se i server di accesso di rete sono configurati per l'invio di traffico RADIUS su porte diverse da quelle predefinite, rimuovere le eccezioni create durante l'installazione dei criteri di RETE in Windows Firewall con sicurezza avanzata e creare eccezioni per le porte utilizzate per il traffico RADIUS.

**Procedura per Windows PowerShell:**

Per eseguire questa procedura usando Windows PowerShell, eseguire Windows PowerShell come amministratore, immettere il cmdlet seguente:

```powershell
Install-WindowsFeature NPAS -IncludeManagementTools
```

**Procedura per Server Manager:**

1.  In Server Manager selezionare **Gestisci**e quindi selezionare **Aggiungi ruoli e funzionalità**.
        Viene avviata l'Aggiunta guidata ruoli e funzionalità.

2.  In prima di iniziare selezionare **Avanti**.

    >[!NOTE] 
    >La pagina **prima di iniziare** dell'aggiunta guidata ruoli e funzionalità non viene visualizzata se in precedenza è stata selezionata l'opzione **Ignora questa pagina per impostazione predefinita** quando è stata eseguita la procedura guidata Aggiungi ruoli e funzionalità.

3.  In selezione tipo di installazione assicurarsi che sia selezionata l'opzione **installazione basata su ruoli o basata su funzionalità** , quindi selezionare **Avanti**.

4.  In selezione server di destinazione verificare che sia selezionata l'opzione **selezionare un server dal pool di server** .

5.  In pool di server, verificare che il computer locale sia selezionato e fare clic su **Avanti**.

6.  In Selezione ruoli server, in **ruoli**, selezionare **servizi di accesso e criteri di rete**. Verrà visualizzata una finestra di dialogo in cui viene chiesto se aggiungere funzionalità necessarie per servizi di accesso e criteri di rete.

7.  Selezionare **Aggiungi funzionalità**, quindi fare clic su **Avanti** .

8.  In Selezione funzionalità selezionare **Avanti**e in servizi di accesso e criteri di rete esaminare le informazioni fornite e quindi fare clic su **Avanti**.

9.  In Selezione servizi ruolo selezionare **Server dei criteri di rete**.

10. Per le funzionalità necessarie per server dei criteri di rete, selezionare **Aggiungi funzionalità**, quindi fare clic su **Avanti**.

11. In Conferma selezioni per l'installazione selezionare **Riavvia automaticamente il server di destinazione, se necessario**.

12. Selezionare **Sì** per confermare la selezione e quindi selezionare **Installa**.
    
    Nella pagina stato dell'installazione viene visualizzato lo stato durante il processo di installazione. Al termine del processo, il messaggio "Installazione completata in *nomecomputer*" viene visualizzato, in cui *nomecomputer* è il nome del computer su cui è installato Server dei criteri di rete.

13. Seleziona **Chiudi**.

## <a name="configure-nps"></a>Configurazione del server dei criteri di rete

Dopo aver installato Server dei criteri di rete, configurare NPS per gestire tutti i compiti di autenticazione, autorizzazione e accounting per la richiesta di connessione ricevuta dal server VPN.

### <a name="register-the-nps-server-in-active-directory"></a>Registrare il server NPS in Active Directory

In questa procedura viene registrato il server in Active Directory in modo che disponga dell'autorizzazione per accedere alle informazioni sull'account utente durante l'elaborazione delle richieste di connessione.

**Procedura**

1.  In Server Manager selezionare **strumenti**e quindi **Server dei criteri di rete**. Si apre la console NPS.

2.  Nella console NPS, fare clic con il pulsante destro del mouse su **NPS (local)** , quindi selezionare **registra server in Active Directory**.
   
     Verrà visualizzata la finestra di dialogo Server dei criteri di rete.

3.  Nella finestra di dialogo Server dei criteri di rete fare clic su **OK** due volte.

Per metodi alternativi per la registrazione di server dei criteri di server, vedere [registrare un server NPS in un dominio di Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### <a name="configure-network-policy-server-accounting"></a>Configurare le funzionalità di accounting del Server dei criteri di rete

In questa procedura configurare l'accounting del server dei criteri di rete utilizzando uno dei tipi di registrazione seguenti:

- **Registrazione degli eventi**. Utilizzato principalmente per il controllo e la risoluzione dei problemi relativi ai tentativi di connessione. È possibile configurare la registrazione degli eventi NPS ottenendo le proprietà del server NPS nella console NPS.

- **Registrazione delle richieste di autenticazione e accounting degli utenti in un file locale**. Utilizzato principalmente per l'analisi della connessione e la fatturazione. Utilizzato anche come strumento di analisi della sicurezza perché fornisce un metodo per tenere traccia dell'attività di un utente malintenzionato dopo un attacco. È possibile configurare la registrazione di file locali utilizzando la configurazione guidata contabilità.

- **Registrazione delle richieste di autenticazione e accounting degli utenti in un database conforme a Microsoft SQL Server XML**. Usato per consentire a più server che eseguono NPS di avere un'origine dati. Offre inoltre i vantaggi derivanti dall'utilizzo di un database relazionale. È possibile configurare la registrazione SQL Server tramite la configurazione guidata contabilità.

Per configurare l'accounting del server dei criteri di rete, vedere [configurare la contabilità server dei criteri di rete](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### <a name="add-the-vpn-server-as-a-radius-client"></a>Aggiungere il server VPN come client RADIUS

Nella sezione [configurare il server di accesso remoto per Always on VPN](vpn-deploy-ras.md) è stato installato e configurato il server VPN. Durante la configurazione del server VPN è stato aggiunto un segreto condiviso RADIUS nel server VPN.

In questa procedura viene usata la stessa stringa di testo segreta condivisa per configurare il server VPN come client RADIUS in server dei criteri di rete. Utilizzare la stessa stringa di testo utilizzata nel server VPN oppure la comunicazione tra il server dei criteri di rete e il server VPN non riesce.

>[!IMPORTANT] 
>Quando si aggiunge alla rete un nuovo server di accesso alla rete, ovvero un server VPN, un punto di accesso wireless, un commutatore di autenticazione o un server di connessione remota, è necessario aggiungere il server come client RADIUS in NPS, in modo che server dei criteri di rete sia in grado di comunicare con il server di accesso alla rete.

**Procedura**

1. Nel server NPS, nella console server dei criteri di server, fare doppio clic su **client e server RADIUS**.

2. Fare clic con il pulsante destro del mouse su **client RADIUS** e scegliere **nuovo**. Verrà visualizzata la finestra di dialogo nuovo client RADIUS.

3. Verificare che la casella di controllo **Abilita questo client RADIUS** sia selezionata.

4. In **nome descrittivo**immettere un nome visualizzato per il server VPN.

5. In **indirizzo (IP o DNS)** immettere l'indirizzo IP o il nome di dominio completo del server NAS.
     
     Se si immette il nome di dominio completo, selezionare **Verifica** se si desidera verificare che il nome sia corretto e che sia mappato a un indirizzo IP valido.

6. In **segreto condiviso**eseguire le operazioni seguenti:

    1. Assicurarsi che sia selezionata l'opzione **manuale** .

    2. Immettere la stringa di testo complessa che è stata immessa anche nel server VPN.

    3. Immettere nuovamente il segreto condiviso in Conferma segreto condiviso.

7. Scegliere **OK**. Il server VPN verrà visualizzato nell'elenco dei client RADIUS configurati nel server dei criteri di rete.

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>Configurare server dei criteri di rete come RADIUS per le connessioni VPN

In questa procedura viene configurato il server dei criteri di rete come server RADIUS nella rete dell'organizzazione. Nel server dei criteri di rete è necessario definire un criterio che consenta solo agli utenti di un gruppo specifico di accedere alla rete aziendale/aziendale tramite il server VPN, quindi solo quando si utilizza un certificato utente valido in una richiesta di autenticazione PEAP.

**Procedura**

1. Nella console server dei criteri di rete, in configurazione standard, verificare che sia selezionata l'opzione **server RADIUS per connessioni remote o VPN** .

2. Selezionare **Configura VPN o connessione remota**.
        
    Verrà visualizzata la procedura guidata Configura VPN o connessione remota.

3. Selezionare **connessioni di rete privata virtuale (VPN)** e fare clic su **Avanti**.

4. In specificare il server VPN o remoto in client RADIUS selezionare il nome del server VPN aggiunto nel passaggio precedente. Ad esempio, se il nome NetBIOS del server VPN è RAS1, selezionare **RAS1**.

5. Selezionare **Avanti**.

6. In Configura metodi di autenticazione completare i passaggi seguenti:

    1. Deselezionare la casella di controllo **Microsoft Encrypted Authentication versione 2 (MS-CHAPv2)** .

    2. Selezionare la casella di controllo **Extensible Authentication Protocol** per selezionarla.

    3. In tipo (in base al metodo di accesso e alla configurazione di rete) **Selezionare Microsoft: PEAP (Protected EAP**), quindi selezionare **Configure (Configura**).
      
        Verrà visualizzata la finestra di dialogo Modifica proprietà EAP protetto.

    4. Selezionare **Rimuovi** per rimuovere il tipo EAP password protetta (EAP-MSCHAP v2).

    5. Selezionare **Aggiungi**. Verrà visualizzata la finestra di dialogo Aggiungi EAP.

    6. Selezionare **Smart Card o altro certificato**, quindi fare clic su **OK**.

    7. Selezionare **OK** per chiudere Modifica proprietà EAP protette.

7. Selezionare **Avanti**.

8. In specificare i gruppi di utenti completare i passaggi seguenti:

    1. Selezionare **Aggiungi**. Verrà visualizzata la finestra di dialogo Seleziona utenti, computer, account servizio o gruppi.

    2. Immettere **utenti VPN**, quindi fare clic su **OK**.

    3. Selezionare **Avanti**.

9. In specificare i filtri IP selezionare **Avanti**.

10. In specificare le impostazioni di crittografia selezionare **Avanti**. Non apportare modifiche.

    Queste impostazioni si applicano solo alle connessioni Microsoft Point-to-Point Encryption (MPPE), che non sono supportate in questo scenario.

11. In specificare un nome dell'area di autenticazione selezionare **Avanti**.

12. Selezionare **fine** per chiudere la procedura guidata.

## <a name="autoenroll-the-nps-server-certificate"></a>Registrare automaticamente il certificato del server NPS

In questa procedura si aggiorna manualmente Criteri di gruppo nel server dei criteri di criteri di locale. Quando Criteri di gruppo Aggiorna, se la registrazione automatica del certificato è configurata e funzionante correttamente, il computer locale viene registrato automaticamente da un'autorità di certificazione (CA).

>[!NOTE]  
>Criteri di gruppo aggiornati automaticamente al riavvio del computer membro del dominio o quando un utente accede a un computer membro del dominio. Inoltre, Criteri di gruppo viene aggiornato periodicamente. Per impostazione predefinita, questo aggiornamento periodico si verifica ogni 90 minuti con un offset casuale di un massimo di 30 minuti.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

**Procedura**

1. Nel server dei criteri di server aprire Windows PowerShell.

2. Al prompt di Windows PowerShell, digitare **gpupdate**, quindi premere INVIO.

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 5. Configurare le impostazioni di DNS e firewall per](vpn-deploy-dns-firewall.md)always on VPN: In questo passaggio viene installato Server dei criteri di rete utilizzando Windows PowerShell o l'Server Manager aggiunta guidata ruoli e funzionalità. Viene inoltre configurato il server dei criteri di rete per gestire tutti i compiti di autenticazione, autorizzazione e accounting per le richieste di connessione ricevute dal server VPN.