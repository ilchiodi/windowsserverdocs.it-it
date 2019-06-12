---
title: Installare e configurare il Server dei criteri di rete
description: Elaborazione del server dei criteri di rete delle richieste di connessione che vengono inviati dal server VPN verifica che l'utente dispone dell'autorizzazione per la connessione, l'identità dell'utente e registra gli aspetti della richiesta di connessione che si è scelto durante la configurazione di accounting RADIUS in Criteri di rete.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: ca52aeeed8c4872e4d9a433c55bddc65a74d9dc0
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749621"
---
# <a name="step-4-install-and-configure-the-network-policy-server-nps"></a>Passaggio 4. Installare e configurare il Server dei criteri di rete (NPS)

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**prossimo:** Passaggio 3. Configurare il server di accesso remoto per VPN Always On](vpn-deploy-ras.md)
- [**prossimo:** Passaggio 5. Configurare le impostazioni di firewall e DNS](vpn-deploy-dns-firewall.md)

In questo passaggio, verrà installato Strumentazione gestione Windows (NPS, Network Policy Server) per l'elaborazione delle richieste di connessione che vengono inviati dal server VPN:

- Eseguire l'autorizzazione per verificare che l'utente disponga dell'autorizzazione per la connessione.
- Eseguire l'autenticazione per verificare l'identità dell'utente.
- Esecuzione di contabilità per registrare gli aspetti della richiesta di connessione che si è scelto durante la configurazione di accounting RADIUS in Criteri di rete.

I passaggi descritti in questa sezione consentono di completare gli elementi seguenti:

1. Nel computer o macchina virtuale che è pianificato per il server NPS e installato in un'organizzazione o alla rete aziendale, è possibile installare NPS.

   >[!TIP]
   >Se si dispone già di uno o più server dei criteri di rete nella rete, non devi eseguire l'installazione del Server dei criteri di rete: in alternativa, è possibile utilizzare questo argomento per aggiornare la configurazione di un server dei criteri di rete esistente.

> [!NOTE]
> È possibile installare il servizio Server dei criteri di rete non in Windows Server Core.

2. Nel server dei criteri di rete aziendali / dell'organizzazione, è possibile configurare criteri di rete per fungere da server RADIUS che elabora le richieste di connessione ricevute dal server VPN.

## <a name="install-network-policy-server"></a>Installare il server dei criteri di rete

In questa procedura per installare dei criteri di rete, usando Windows PowerShell o il Server Manager Aggiungi ruoli e funzionalità guidata. Server dei criteri di rete è un servizio ruolo del ruolo server Servizi di accesso e criteri di rete.

>[!TIP]
>Per impostazione predefinita, Server dei criteri di rete rimane in ascolto del traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 di tutte le schede di rete installate. Quando si installa NPS e si abilita Windows Firewall con sicurezza avanzata, le eccezioni del firewall per queste porte create automaticamente per il traffico IPv4 e IPv6. Se i server di accesso di rete sono configurati per l'invio di traffico RADIUS su porte diverse da quelle predefinite, rimuovere le eccezioni create durante l'installazione dei criteri di RETE in Windows Firewall con sicurezza avanzata e creare eccezioni per le porte utilizzate per il traffico RADIUS.

**Procedure per Windows PowerShell:**

Per eseguire questa procedura mediante Windows PowerShell, eseguire Windows PowerShell come amministratore, immettere il cmdlet seguente:

```powershell
Install-WindowsFeature NPAS -IncludeManagementTools
```

**Procedura per Server Manager:**

1.  In Server Manager, selezionare **Manage**, quindi selezionare **Aggiungi ruoli e funzionalità**.
        Viene avviata l'Aggiunta guidata ruoli e funzionalità.

2.  Nella prima di iniziare, selezionare **successivo**.

    >[!NOTE] 
    >Il **prima di iniziare** pagina l'aggiunta guidata ruoli e funzionalità non viene visualizzata se è stato selezionato in precedenza **Ignora questa pagina per impostazione predefinita** quando è stata eseguita l'aggiunta guidata ruoli e funzionalità.

3.  In Selezione tipo di installazione, assicurarsi che **installazione basata su ruoli o basata su funzionalità** sia selezionata, quindi selezionare **successivo**.

4.  In Selezione server di destinazione, assicurarsi che **selezionare un server dal pool di server** sia selezionata.

5.  Nel Pool di Server, verificare che il computer locale sia selezionato e selezionare **successivo**.

6.  In Selezione ruoli Server, in **ruoli**, selezionare **servizi di accesso e criteri di rete**. Viene aperta una finestra di dialogo che chiede se è necessario aggiungere le funzionalità necessarie per servizi di accesso e criteri di rete.

7.  Selezionare **Aggiungi funzionalità**, quindi selezionare **successivo**

8.  In Selezione funzionalità selezionare **successivo**e in servizi di accesso e criteri di rete, esaminare le informazioni fornite, quindi selezionare **successivo**.

9.  Nella finestra Servizi selezionare di ruolo, selezionare **Server dei criteri di rete**.

10. Per le funzionalità necessarie per Server dei criteri di rete, selezionare **Aggiungi funzionalità**, quindi selezionare **successivo**.

11. In Conferma selezioni per l'installazione, selezionare **riavvia automaticamente il server di destinazione se necessario**.

12. Selezionare **Yes** per confermare la selezionato e quindi selezionare **installare**.
    
    Nella pagina di stato di installazione visualizza lo stato durante il processo di installazione. Al termine del processo, il messaggio "Installazione completata in *nomecomputer*" viene visualizzato, in cui *nomecomputer* è il nome del computer su cui è installato Server dei criteri di rete.

13. Seleziona **Chiudi**.

## <a name="configure-nps"></a>Configurazione del server dei criteri di rete

Dopo aver installato NPS, configurato dei criteri di rete per gestire tutti i autenticazione, autorizzazione e richiedono compiti di contabilità per la connessione riceve dal server VPN.

### <a name="register-the-nps-server-in-active-directory"></a>Registrare il Server NPS in Active Directory

In questa procedura registrare il server in Active Directory in modo che disponga dell'autorizzazione per accedere alle informazioni dell'account utente durante l'elaborazione delle richieste di connessione.

**Procedura:**

1.  In Server Manager, selezionare **degli strumenti**, quindi selezionare **Server dei criteri di rete**. Apre la console Criteri di rete.

2.  Nella console Criteri di rete, fare doppio clic su **dei criteri di rete (locale)** , quindi selezionare **Registra server in Active Directory**.
   
     Verrà visualizzata la finestra di dialogo Server dei criteri di rete.

3.  Nella finestra di dialogo Server dei criteri di rete, selezionare **OK** due volte.

Per metodi alternativi per la registrazione dei criteri di rete, vedere [registrare un Server NPS in un dominio Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### <a name="configure-network-policy-server-accounting"></a>Configurare le funzionalità di accounting del Server dei criteri di rete

In questa procedura Configura rete Accounting Server dei criteri usando uno dei seguenti tipi di registrazione:

- **La registrazione degli eventi**. Utilizzato principalmente per il controllo e risoluzione dei problemi relativi a tentativi di connessione. È possibile configurare la registrazione per ottenere le proprietà del server dei criteri di rete nella console di NPS eventi dei criteri di rete.

- **Registrazione delle richieste di accounting e autenticazione degli utenti in un file locale**. Utilizzato principalmente per scopi di analisi e la fatturazione di connessione. Usato anche come uno strumento di analisi di sicurezza perché si offre un metodo di verifica dell'attività di un utente malintenzionato dopo un attacco. È possibile configurare la registrazione di file locale usando la configurazione guidata Accounting.

- **Registrazione delle richieste di accounting e autenticazione degli utenti a un database Microsoft SQL Server compatibile con XML**. Utilizzato per consentire a più server dei criteri di rete per avere una sola origine dati. Fornisce inoltre i vantaggi dell'uso di un database relazionale. È possibile configurare la registrazione di SQL Server usando la configurazione guidata Accounting.

Per configurare l'Accounting Server criteri di rete, vedere [configurare del Accounting Server dei criteri di rete](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### <a name="add-the-vpn-server-as-a-radius-client"></a>Aggiungere il Server VPN come Client RADIUS

Nel [configurare il Server di accesso remoto per VPN Always On](vpn-deploy-ras.md) sezione, è installato e configurato il server VPN. Durante la configurazione del server VPN, è stato aggiunto un segreto condiviso RADIUS del server VPN.

In questa procedura, si usa la stessa stringa di testo segreto condiviso per configurare il server VPN come client RADIUS in Criteri di rete. Utilizzare la stessa stringa di testo che è stato usato nel server VPN, oppure si verifica un errore di comunicazione tra il server dei criteri di rete e server VPN.

>[!IMPORTANT] 
>Quando si aggiunge un nuovo server di accesso di rete (VPN server, punto di accesso wireless, switch di autenticazione o server remoto) alla rete, è necessario aggiungere il server come client RADIUS in Criteri di rete in modo che riconosca dei criteri di rete e può comunicare con il server di accesso di rete.

**Procedura:**

1. Nel server NPS, nella console Criteri di rete, fare doppio clic su **client RADIUS e server**.

2. Fare doppio clic su **client RADIUS** e selezionare **New**. Verrà visualizzata la finestra di dialogo Nuovo Client RADIUS.

3. Verificare che il **attiva questo client RADIUS** casella di controllo è selezionata.

4. Nelle **soprannome**, immettere un nome visualizzato per il server VPN.

5. Nelle **indirizzo (IP o DNS)** , immettere il FQDN o indirizzo IP di NAS.
     
     Se si immette il nome di dominio completo, selezionare **Verify** se si desidera verificare che il nome sia corretto e che esegue il mapping a un indirizzo IP valido.

6. Nelle **segreto condiviso**, eseguire:

    1. Assicurarsi che **manuale** sia selezionata.

    2. Immettere la stringa di testo in grassetto immesso anche nel server VPN.

    3. Immettere nuovamente il segreto condiviso nella conferma segreto condiviso.

7. Scegliere **OK**. Il Server VPN viene visualizzato nell'elenco dei computer client RADIUS configurato nel server NPS.

## <a name="configure-nps-as-a-radius-for-vpn-connections"></a>Configurare i criteri di rete come un raggio per le connessioni VPN

In questa procedura configurare i criteri di rete come server RADIUS nella rete dell'organizzazione. Su NPS, è necessario definire un criterio che consenta solo gli utenti in un gruppo specifico di accedere alla rete aziendale o dell'organizzazione tramite il Server VPN - e quindi solo quando si usa un certificato utente valido in una richiesta di autenticazione PEAP.

**Procedura:**

1. Nella console di NPS, nella configurazione Standard, assicurarsi che **server RADIUS per connessioni remote o VPN** sia selezionata.

2. Selezionare **configurare connessioni remote o VPN**.
        
    Apre la procedura guidata configurazione VPN o connessioni remote.

3. Selezionare **le connessioni di rete privata virtuale (VPN)** e selezionare **successivo**.

4. In specificare connessioni remote o Server VPN, nel computer client RADIUS, selezionare il nome del Server VPN a cui è stato aggiunto nel passaggio precedente. Ad esempio, se il nome NetBIOS del server VPN RAS1, selezionare **RAS1**.

5. Selezionare **Avanti**.

6. Nel configurare metodi di autenticazione, completare i passaggi seguenti:

    1. Cancella il **autenticazione crittografata Microsoft versione 2 (MS-CHAP v2)** casella di controllo.

    2. Selezionare il **Extensible Authentication Protocol** casella di controllo per selezionarla.

    3. In tipo (in base al metodo di configurazione di rete e accesso) selezionare **Microsoft: PEAP (Protected EAP)** , quindi selezionare **configura**.
      
        Verrà visualizzata la finestra di dialogo Modifica proprietà PEAP.

    4. Selezionare **rimuovere** per rimuovere il tipo EAP Password protetta (EAP-MSCHAP v2).

    5. Selezionare **Aggiungi**. Verrà visualizzata la finestra di dialogo Aggiunta EAP.

    6. Selezionare **Smart Card o altro certificato**, quindi selezionare **OK**.

    7. Selezionare **OK** chiudere Modifica proprietà PEAP.

7. Selezionare **Avanti**.

8. Specificare gruppi di utenti, completare i passaggi seguenti:

    1. Selezionare **Aggiungi**. Verrà aperta la finestra di dialogo Seleziona utenti, computer, account del servizio o gruppi.

    2. Immettere **gli utenti VPN**, quindi selezionare **OK**.

    3. Selezionare **Avanti**.

9. Nella specifica filtri IP, selezionare **successivo**.

10. Specificare le impostazioni di crittografia, selezionare **successivo**. Non apportare modifiche.

    Queste impostazioni si applicano solo alle connessioni Microsoft Point to Point Encryption (MPPE), che non supporta questo scenario.

11. In specificare un nome dell'area di autenticazione, selezionare **successivo**.

12. Selezionare **fine** per chiudere la procedura guidata.

## <a name="autoenroll-the-nps-server-certificate"></a>Registrare automaticamente il certificato Server NPS

In questa procedura aggiornare criteri di gruppo nel server NPS locale manualmente. Quando gli aggiornamenti di criteri di gruppo, se è configurata la registrazione automatica del certificato e funziona correttamente, il computer locale è auto-registrato un certificato dall'autorità di certificazione (CA).

>[!NOTE]  
>Criteri di gruppo aggiornato automaticamente quando si riavvia il computer membro del dominio o quando un utente accede a un computer membro del dominio. Inoltre, criteri di gruppo viene aggiornato periodicamente. Per impostazione predefinita, questo aggiornamento periodico viene eseguito ogni 90 minuti con un offset casuale fino a 30 minuti.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

**Procedura:**

1. In NPS, aprire Windows PowerShell.

2. Al prompt di Windows PowerShell, digitare **gpupdate**, quindi premere INVIO.

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 5. Configurare le impostazioni DNS e del firewall per VPN Always On](vpn-deploy-dns-firewall.md): In questo passaggio per installare Server dei criteri di rete (NPS), usando Windows PowerShell o il Server Manager Aggiungi ruoli e funzionalità guidata. È inoltre configurare criteri di rete per gestire l'autenticazione, autorizzazione e compiti di contabilità per la connessione tutte le richieste ricevute dal server VPN.