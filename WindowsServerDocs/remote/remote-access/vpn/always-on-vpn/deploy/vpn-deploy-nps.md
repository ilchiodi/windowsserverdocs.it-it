---
title: Installare e configurare il Server dei criteri di rete
description: L'elaborazione di server dei criteri di rete di richieste di connessione che vengono inviati dal server VPN verifica che l'utente dispone dell'autorizzazione per la connessione, l'identità dell'utente e registra gli aspetti della richiesta di connessione che hai scelto quando hai configurato l'accounting RADIUS in Criteri di rete.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: ca53ef28497a78f264c60ac1132f721fb6e01c15
ms.sourcegitcommit: 9c142ad4d4321baf328707f29fa104247ff5149a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2019
ms.locfileid: "9035767"
---
# Passaggio 4. Installare e configurare Server dei criteri di rete (NPS)

>   Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Successivo:** passaggio 3. Configurare il Server di accesso remoto per VPN Always On](vpn-deploy-ras.md)<br>
& #187;  [ **Successivo:** passaggio 5. Configurare le impostazioni di Firewall e DNS](vpn-deploy-dns-firewall.md)


In questo passaggio, installare Server dei criteri di rete (NPS) per l'elaborazione delle richieste di connessione che vengono inviati dal server VPN:

- Eseguire l'autorizzazione per verificare che l'utente abbia l'autorizzazione per la connessione.
- Eseguire l'autenticazione per verificare l'identità dell'utente.
- Esecuzione di contabilità per registrare gli aspetti della richiesta di connessione che hai scelto quando hai configurato l'accounting RADIUS in Criteri di rete.

I passaggi descritti in questa sezione consentono di completare gli elementi seguenti:

1.  Il computer o macchina virtuale che pianificati per il server dei criteri di rete e installato in una rete azienda o dell'organizzazione, è possibile installare dei criteri di rete.

   >[!TIP] 
   >Se hai già uno o più server dei criteri di rete nella rete, non è necessaria eseguire l'installazione di Server dei criteri di rete - al contrario, è possibile utilizzare questo argomento per aggiornare la configurazione di un server dei criteri di rete esistente.

>[!NOTE]  
Non puoi installare il servizio di Server dei criteri di rete in Windows Server Core.

2.  Nel server dei criteri di rete aziendale o dell'organizzazione, è possibile configurare dei criteri di rete per eseguire come server RADIUS che elabora le richieste di connessione ricevute dal server VPN.

## Installare il server dei criteri di rete

In questa procedura, installare dei criteri di rete usando Windows PowerShell o il Server Manager Aggiungi ruoli e funzionalità guidata. Dei criteri di rete è un servizio ruolo del ruolo server Servizi di accesso e criteri di rete.

>[!TIP] 
>Per impostazione predefinita, è in ascolto dei criteri di rete per il traffico RADIUS sulle porte 1812, 1813, 1645 e 1646 su tutte le schede di rete installate. Quando si installa dei criteri di rete e attivare Windows Firewall con sicurezza avanzata, le eccezioni del firewall per queste porte vengono create automaticamente per il traffico IPv4 e IPv6. Se i server di accesso di rete sono configurati per inviare il traffico RADIUS tramite le porte diverso da queste impostazioni predefinite, rimuovere le eccezioni in Windows Firewall con protezione avanzata create durante l'installazione dei criteri di rete e creare eccezioni per le porte da utilizzare per Traffico RADIUS.

**Procedura per Windows PowerShell:**

Per eseguire questa procedura tramite Windows PowerShell, Esegui Windows PowerShell come amministratore, digita il comando seguente e quindi premi INVIO.

`Install-WindowsFeature NPAS -IncludeManagementTools`

**Procedura per Server Manager:**

1.  In Server Manager, fare clic su **Gestisci**e fai clic su **Aggiungi ruoli e funzionalità**. <p>Verrà visualizzata l'aggiunta guidata ruoli e funzionalità.

2.  Nella prima di iniziare, fai clic su **Avanti**.

    >[!NOTE] 
    >La pagina **Prima di iniziare** dell'Aggiunta guidata ruoli e funzionalità non viene visualizzata se in precedenza selezionato **Ignora questa pagina per impostazione predefinita** quando è stato eseguito l'aggiunta guidata ruoli e funzionalità.

1.  In Seleziona tipo di installazione, assicurati che **l'installazione basato sui ruoli o basata su funzionalità** viene selezionato e fare clic su **Avanti**.

2.  Nella selezione server di destinazione, assicurati che sia selezionata **Selezionare un server dal pool di server** .

3.  Nel Pool di Server, assicurarsi che sia selezionato nel computer locale e fare clic su **Avanti**.

4.  Nella selezione ruoli Server, **ruoli**, seleziona **servizi di accesso e criteri di rete**. Una finestra di dialogo viene visualizzata se è necessario aggiungere le funzionalità necessarie per accedere ai servizi e i criteri di rete.

5.  Fai clic su **Aggiungi funzionalità**e fare clic su **Avanti**

6.  In selezionare le funzionalità, fare clic su **Avanti**e rivedere le informazioni fornite in servizi di accesso e criteri di rete e fai clic su **Avanti**.

7.  In servizi di selezione ruolo, fai clic sul **Server dei criteri di rete**.

8.  Per le funzionalità necessarie per il Server dei criteri di rete, fai clic su **Aggiungi funzionalità** e, fare clic su **Avanti**.

9.  Nella finestra di conferma selezioni per l'installazione, fai clic su **riavviare il server di destinazione automaticamente se richiesto**.

10. Fai clic su **Sì** per confermare la selezionato e quindi fai clic su **Installa**. <p>La pagina di avanzamento di installazione visualizza lo stato durante il processo di installazione. Al termine del processo, il messaggio "installazione ha avuto esito positivo in *ComputerName"* viene visualizzato, in cui *ComputerName* è il nome del computer su cui è stato installato il Server dei criteri di rete.

11. Fai clic su **Chiudi**.

## Configurazione dei criteri di rete

Dopo l'installazione dei criteri di rete, configurare dei criteri di rete per gestire tutti i autenticazione, l'autorizzazione e compiti contabilità per la connessione viene effettuata richiesta ricevuti dal server VPN.

### Registrare il Server dei criteri di rete in Active Directory

In questa procedura, registrare il server in Active Directory in modo che abbia l'autorizzazione per accedere a informazioni sull'account utente durante l'elaborazione delle richieste di connessione.

**Procedura:**

1.  In Server Manager, fai clic su **Strumenti**e quindi fai clic su **Server dei criteri di rete**. Si apre la console dei criteri di rete.

2.  Nella console di criteri di rete, fai **dei criteri di rete (locale)** e fai clic su **Register server in Active Directory** per selezionarlo.<p>Viene visualizzata la finestra di dialogo di Server dei criteri di rete.

3.  Nella finestra di dialogo Server dei criteri di rete, fai clic su **OK** due volte.

Per i metodi alternativi per la registrazione dei criteri di rete, vedi [registrare un Server dei criteri di rete in un dominio Active Directory](../../../../../networking/technologies/nps/nps-manage-register.md).

### Configurare le funzionalità di accounting del Server dei criteri di rete

In questa procedura, Configura rete Accounting Server dei criteri utilizzando uno dei seguenti tipi di registrazione:

-   **Registrazione degli eventi**. Usata principalmente per il controllo e risoluzione dei problemi di tentativi di connessione. È possibile configurare la registrazione, otteniamo le proprietà di server dei criteri di rete nella console di criteri di rete eventi dei criteri di rete.

-   **Registrazione delle richieste di accounting in un file locale e l'autenticazione utente**. Usata principalmente per scopi di fatturazione e di analisi di connessione. Usato anche come uno strumento di analisi di sicurezza perché fornisce un metodo di verifica dell'attività di un utente malintenzionato dopo un attacco. È possibile configurare la registrazione di file locale tramite la configurazione guidata Accounting.

-   **Registrazione delle richieste di accounting a un database Microsoft SQL Server XML conforme e l'autenticazione utente**. Usato per consentire a più server dei criteri di rete di avere un'origine dati. Fornisce inoltre i vantaggi dell'uso di un database relazionale. È possibile configurare la registrazione di SQL Server tramite la configurazione guidata Accounting.

Per configurare l'Accounting Server dei criteri di rete, vedi [Configurare del Accounting Server dei criteri di rete](../../../../../networking/technologies/nps/nps-accounting-configure.md).

### Aggiungere il Server VPN come Client RADIUS

Nella sezione, [Configura il Server di accesso remoto per VPN Always On](vpn-deploy-ras.md) è installato e configurato il server VPN. Durante la configurazione del server VPN, è stato aggiunto un segreto condiviso RADIUS nel server VPN. 

In questa procedura, utilizzare la stessa stringa di testo segreto condiviso per configurare il server VPN come client RADIUS in Criteri di rete. Usare la stessa stringa di testo che hai usato nel server VPN o la comunicazione tra il server dei criteri di rete VPN non riesce.

>[!IMPORTANT] 
>Quando si aggiunge un nuovo server di accesso di rete (VPN server, il punto di accesso wireless, il commutatore o server remoto) alla rete, è necessario aggiungere il server come client RADIUS in Criteri di rete in modo che dei criteri di rete è a conoscenza del e possa comunicare con il server di accesso di rete.

**Procedura:**

1.  Nel server dei criteri di rete, nella console di criteri di rete, fai doppio clic sul **client e server RADIUS**.

2.  Fai **Client RADIUS** e fai clic di **Nuovo**. Viene visualizzata la finestra di dialogo Nuovo Client RADIUS.

3.  Verificare che sia selezionata la casella di controllo **abilitare questo client RADIUS** .

4.  **Nome descrittivo**, Immetti un nome visualizzato per il server VPN.

5.  **Indirizzo (IP o DNS)**, Immetti il FQDN o l'indirizzo IP NAS.<p>Se immetti il nome FQDN, fare clic su **verifica** se vuoi verificare che il nome sia corretto e che esegue il mapping a un indirizzo IP valido.

6.  Nella finestra **segreto condiviso**, eseguire:

    1.  Assicurati che sia selezionata **manuale** .

    2.  Immetti la stringa di testo avanzata che è stato immesso anche il server VPN.

    3.  Digitarla di nuovo il segreto condiviso in Conferma segreto condiviso.

7.  Scegli **OK**. Il Server VPN viene visualizzata nell'elenco dei client RADIUS configurati nel server dei criteri di rete.

## Configurazione dei criteri di rete come un raggio per le connessioni VPN

In questa procedura, configurare dei criteri di rete come server RADIUS sulla rete dell'organizzazione. In Criteri di rete, è necessario definire un criterio che consente solo agli utenti in un gruppo specifico di accedere alla rete dell'organizzazione o aziendali tramite il Server VPN - e quindi solo quando si utilizza un certificato utente valido in una richiesta di autenticazione PEAP.

**Procedura:**

1.  Nella console di criteri di rete, nella configurazione Standard, assicurati che sia selezionato **il server RADIUS per le connessioni VPN o remote** .

2.  Fare clic su **Configura VPN o remote**.<p>Si apre la procedura guidata configurazione VPN o remote.

3.  Fai clic su **connessioni di rete privata virtuale (VPN)** e fare clic su **Avanti**.

4.  Specificare remote o Server VPN, nel client RADIUS, seleziona il nome del Server VPN che hai aggiunto nel passaggio precedente.<p>Ad esempio, se il nome NetBIOS del server VPN è RAS1, fai clic su **RAS1**.

5.  Fai clic su **Avanti**.

6.  Configurare metodi di autenticazione, completa i passaggi seguenti:

    1.  Deseleziona la casella di controllo di **Autenticazione crittografata Microsoft versione 2 (MS-CHAPv2)** .

    2.  Fai clic sulla casella di controllo **Extensible Authentication Protocol** per selezionarlo.

    3.  Nel tipo (in base al metodo di configurazione di accesso e rete), fai clic su **Microsoft: Protected EAP (PEAP)**, fare clic su **Configura**.<p>Viene visualizzata la finestra di dialogo Modifica proprietà PEAP.

    4.  Fai clic su **Rimuovi** per rimuovere il tipo EAP (EAP-MS-CHAP v2) Password protetta.

    5.  Fai clic su **Aggiungi**. Si apre la finestra di dialogo Aggiungi EAP.

    6.  Fai clic su **Smart Card o un altro certificato**e fai clic su **OK**.

    7.  Fai clic su **OK** per chiudere Modifica proprietà PEAP.

7.  Fai clic su **Avanti**.

8.  Nella finestra di specificare gruppi di utenti, completare i passaggi seguenti:

    1.  Fai clic su **Aggiungi**. Si apre la finestra di dialogo Selezione utenti, computer, gli account del servizio o gruppi.

    2.  Digita **Gli utenti della VPN** e fai clic su **OK**.

    3.  Fai clic su **Avanti**.

9.  Specificare IP filtri, fai clic su **Avanti**.

10. Nella finestra di specificare le impostazioni di crittografia, fare clic su **Avanti**. Non apporta le modifiche.<p>Queste impostazioni si applicano solo per le connessioni Microsoft Point to Point Encryption (MPPE), che non supporta questo scenario.

11. Nella finestra di specificare un nome di aree di autenticazione, fare clic su **Avanti**.

12. Fai clic su **Fine** per chiudere la procedura guidata.

## Registrazione automatica certificato di Server dei criteri di rete

In questa procedura, aggiornare criteri di gruppo nel server dei criteri di rete locale manualmente. Quando gli aggiornamenti di criteri di gruppo, se è configurata la registrazione automatica del certificato e funziona correttamente, il computer locale è registrato automaticamente un certificato dall'autorità di certificazione (CA).

>[!NOTE]  
>Criteri di gruppo aggiornato automaticamente quando si riavvia il computer membro del dominio o quando un utente accede a un computer membro del dominio. Inoltre, criteri di gruppo si aggiorna periodicamente. Per impostazione predefinita, questo aggiornamento periodico si verifica ogni 90 minuti con una variazione casuale di fino a 30 minuti.

Appartenenza al gruppo **Administrators**, o l'equivalente, è il requisito minimo necessario per completare questa procedura.

**Procedura:**

1.  In Criteri di rete, Apri Windows PowerShell.

2.  Al prompt di Windows PowerShell, digitare **gpupdate**e quindi premi INVIO.

## Passaggio successivo
[Passaggio 5. Configurare le impostazioni DNS e firewall per VPN Always On](vpn-deploy-dns-firewall.md): In questo passaggio, installare Server dei criteri di rete (NPS) usando Windows PowerShell o il Server Manager Aggiungi ruoli e funzionalità guidata. Anche configurare dei criteri di rete per gestire l'autenticazione, autorizzazione e compiti contabilità per la connessione tutte le richieste che riceve dal server VPN.



---

