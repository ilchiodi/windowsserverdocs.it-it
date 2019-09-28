---
title: Configurare le funzionalità di accounting del Server dei criteri di rete
description: In questo argomento vengono fornite informazioni sul file di testo e la registrazione SQL Server per server dei criteri di rete in Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.date: 05/25/2018
ms.openlocfilehash: 0c154d4d4534f4c343107eecd158974b92903e39
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405562"
---
# <a name="configure-network-policy-server-accounting"></a>Configurare le funzionalità di accounting del Server dei criteri di rete

Sono disponibili tre tipi di registrazione per server \(dei criteri di rete NPS:\)

- **Registrazione degli eventi**. Utilizzato principalmente per il controllo e la risoluzione dei problemi relativi ai tentativi di connessione. È possibile configurare la registrazione degli eventi NPS ottenendo le proprietà NPS nella console NPS.

- **Registrazione delle richieste di autenticazione e accounting degli utenti in un file locale**. Utilizzato principalmente per l'analisi della connessione e la fatturazione. Utile anche come strumento di analisi della sicurezza perché fornisce un metodo per tenere traccia dell'attività di un utente malintenzionato dopo un attacco. È possibile configurare la registrazione di file locali utilizzando la configurazione guidata contabilità.

- **Registrazione delle richieste di autenticazione e accounting degli utenti in un database conforme a Microsoft SQL Server XML**. Usato per consentire a più server che eseguono NPS di avere un'origine dati. Offre inoltre i vantaggi derivanti dall'utilizzo di un database relazionale. È possibile configurare la registrazione SQL Server tramite la configurazione guidata contabilità.

## <a name="use-the-accounting-configuration-wizard"></a>Utilizzare la configurazione guidata contabilità

Utilizzando la configurazione guidata contabilità, è possibile configurare le quattro impostazioni di contabilità seguenti:

- **Solo registrazione SQL**. Utilizzando questa impostazione, è possibile configurare un collegamento dati a una SQL Server che consente a NPS di connettersi e inviare i dati di contabilità a SQL Server. Inoltre, la procedura guidata può configurare il database nel SQL Server per garantire la compatibilità del database con la registrazione di SQL Server NPS.
- **Solo registrazione del testo**. Utilizzando questa impostazione, è possibile configurare NPS per la registrazione dei dati di contabilità in un file di testo.
- **Registrazione parallela**. Utilizzando questa impostazione, è possibile configurare il SQL Server collegamento dati e il database. È inoltre possibile configurare la registrazione di file di testo in modo che i server dei criteri di database vengano registrati simultaneamente nel file di testo e nel database SQL Server 
- **Registrazione SQL con backup**. Utilizzando questa impostazione, è possibile configurare il SQL Server collegamento dati e il database. Inoltre, è possibile configurare la registrazione di file di testo utilizzata da server dei criteri di accesso in caso di errore di SQL Server registrazione.

Oltre a queste impostazioni, sia SQL Server registrazione che la registrazione del testo consentono di specificare se il server dei criteri di accesso continua a elaborare le richieste di connessione in caso di errore di registrazione. È possibile specificare questa impostazione nella **sezione azione errori di registrazione** in proprietà registrazione file locale, nelle proprietà di registrazione di SQL Server e durante l'esecuzione della configurazione guidata contabilità.

### <a name="to-run-the-accounting-configuration-wizard"></a>Per eseguire la configurazione guidata contabilità

Per eseguire la configurazione guidata contabilità, attenersi alla procedura seguente:

1. Aprire la console server dei criteri di server o lo snap-in di Microsoft Management Console (MMC) NPS.
2. Nell'albero della console fare clic su **Contabilità**.
3. Nel riquadro dei dettagli, in **Contabilità**, fare clic su **Configura contabilità**.

## <a name="configure-nps-log-file-properties"></a>Configurare le proprietà del file di log di NPS

È possibile configurare server dei criteri di rete per eseguire la contabilità Remote Authentication Dial-In User Service (RADIUS) per le richieste di autenticazione utente, i messaggi di accettazione dell'accesso, i messaggi di rifiuto di accesso, le richieste e le risposte di accounting e periodici aggiornamenti dello stato. È possibile utilizzare questa procedura per configurare i file di log in cui si desidera archiviare i dati di contabilità.

Per ulteriori informazioni sull'interpretazione dei file di log, vedere [interpretare i file di log del formato del database NPS](https://technet.microsoft.com/library/cc771748.aspx).

Per evitare che i file di log compilano l'unità disco rigido, è consigliabile mantenerli in una partizione separata dalla partizione di sistema. Di seguito vengono fornite ulteriori informazioni sulla configurazione dell'accounting per NPS:

- Per inviare i dati del file di log per la raccolta da parte di un altro processo, è possibile configurare NPS per la scrittura in un named pipe. Per utilizzare named pipe, impostare la cartella del file di \\log su \\.\pipe o ComputerName\pipe. Il programma server named pipe crea una named pipe denominata \\.\pipe\iaslog.log per accettare i dati. In crea un nuovo file di log della finestra di dialogo Proprietà file locale selezionare mai (dimensioni file illimitate) quando si utilizzano named pipe.

- È possibile creare la directory dei file di registro usando le variabili di ambiente di sistema, anziché le variabili utente, ad esempio% SystemDrive%,% SystemRoot% e% windir%. Il percorso seguente, ad esempio, utilizzando la variabile di ambiente% windir%, individua il file di log nella directory di sistema nella sottocartella \System32\Logs (ovvero%windir%\System32\Logs @ no__t-0.

- Il cambio di formati di file di log non comporta la creazione di un nuovo log. Se si modificano i formati dei file di log, il file attivo al momento della modifica conterrà una combinazione dei due formati (i record all'inizio del log avranno il formato precedente e i record alla fine del log avranno il nuovo formato).

- Se l'accounting RADIUS ha esito negativo a causa di un'unità disco rigido completa o di altre cause, NPS interrompe l'elaborazione delle richieste di connessione impedendo agli utenti di accedere alle risorse di rete.

- Server dei criteri di database consente di accedere a un® Microsoft SQL Server™ database, oltre che alla registrazione in un file locale o al posto di tale funzionalità.

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** .


### <a name="to-configure-nps-log-file-properties"></a>Per configurare le proprietà del file di log di NPS

1. Aprire la console server dei criteri di server o lo snap-in di Microsoft Management Console (MMC) NPS.
2. Nell'albero della console fare clic su **Contabilità**.
3. Nel riquadro dei dettagli, in **Proprietà file di registro**, fare clic su **modifica proprietà file di registro**. Verrà visualizzata la finestra di dialogo **Proprietà file di registro** .
4. Nelle **proprietà del file di log**, nella scheda **Impostazioni** , in **registra le informazioni seguenti**, assicurarsi di scegliere di registrare informazioni sufficienti per ottenere gli obiettivi di contabilità. Se, ad esempio, i log devono eseguire la correlazione della sessione, selezionare tutte le caselle di controllo.
5. In **azione registrazione non riuscita**selezionare **se la registrazione ha esito negativo, eliminare le richieste di connessione** se si desidera che NPS interrompa l'elaborazione dei messaggi di richiesta di accesso quando i file di log sono pieni o non disponibili per qualche motivo. Se si desidera che NPS continui a elaborare le richieste di connessione in caso di errore di registrazione, non selezionare questa casella di controllo.
6. Nella finestra di dialogo **Proprietà file di registro** fare clic sulla scheda **file di log** .
7. Nella scheda **file di log** , in **directory**, digitare il percorso in cui archiviare i file di log di NPS. Il percorso predefinito è la cartella Systemroot\system32\logfiles.<br>Se non si specifica un'istruzione percorso completo nella **directory del file di log**, viene utilizzato il percorso predefinito. Ad esempio, se si digita **NPSLogFile** nella **directory del file di log**, il file si trova in%systemroot%\System32\NPSLogFile.
8. In **formato**fare clic su **DTS conforme**. Se si preferisce, è possibile selezionare invece un formato di file legacy, ad **esempio \(legacy\) ODBC** o legacy **IAS \(\)** .<br>I tipi di file legacy **ODBC** e **IAS** contengono un subset delle informazioni inviate dal server dei criteri di gruppo al relativo database SQL Server. Il formato XML del tipo di file **conforme a DTS** è identico al formato XML utilizzato da NPS per importare dati nel database SQL Server. Pertanto, il formato di file **conforme a DTS** fornisce un trasferimento più efficiente e completo dei dati nel database di SQL Server standard per NPS.
9. In **Crea un nuovo file di log**, per configurare NPS per l'avvio di nuovi file di log a intervalli specificati, fare clic sull'intervallo che si desidera utilizzare:
    - Per un'attività di registrazione e volume di transazioni complesse, fare clic su **giornaliera**.
    - Per volumi di transazioni e attività di registrazione minori, fare clic su **settimanale** o **mensile**.
    - Per archiviare tutte le transazioni in un unico file di log, fare clic su **dimensioni \(\)file mai illimitate**.
    - Per limitare le dimensioni di ogni file di log, fare clic su **quando il file di log raggiunge questa dimensione**, quindi digitare le dimensioni del file, dopo la creazione di un nuovo log. Le dimensioni predefinite sono pari a 10 megabyte (MB).
10. Se si desidera che il server dei criteri di ricerca elimini i vecchi file di log per creare spazio su disco per i nuovi file di log quando il disco rigido è vicino alla capacità, assicurarsi che **se il disco è pieno, eliminare i file di registro precedenti** . Questa opzione non è disponibile, tuttavia, se il valore di **Crea un nuovo file di log** non è **mai \(illimitato\)** , le dimensioni del file. Inoltre, se il file di log meno recente è il file di log corrente, non viene eliminato.

## <a name="configure-nps-sql-server-logging"></a>Configurare la registrazione SQL Server NPS

È possibile utilizzare questa procedura per registrare i dati di accounting RADIUS in un database locale o remoto che esegue Microsoft SQL Server.

>[!NOTE]
>NPS formatta i dati di contabilità come un documento XML che invia alla **report_event** stored procedure nel database di SQL Server designato in NPS. Per il corretto funzionamento della registrazione SQL Server, è necessario disporre di una stored procedure denominata **report_event** nel database SQL Server che possa ricevere e analizzare i documenti XML da NPS.

Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo Domain Admins o a un gruppo equivalente.

### <a name="to-configure-sql-server-logging-in-nps"></a>Per configurare la registrazione SQL Server in NPS

1. Aprire la console server dei criteri di server o lo snap-in di Microsoft Management Console (MMC) NPS.
2. Nell'albero della console fare clic su **Contabilità**.
3. Nel riquadro dei dettagli, in **SQL Server proprietà registrazione**, fare clic su **modifica SQL Server proprietà registrazione**. Verrà visualizzata la finestra di dialogo **Proprietà registrazione SQL Server** .
4. In **registra le informazioni seguenti**selezionare le informazioni che si desidera registrare: 
    - Per registrare tutte le richieste di contabilità, fare clic su **richieste di contabilità**.
    - Per registrare le richieste di autenticazione, fare clic su **richieste di autenticazione**.
    - Per registrare lo stato di accounting periodico, fare clic su **stato di accounting periodico**.
    - Per registrare lo stato periodico, ad esempio le richieste di contabilità provvisoria, fare clic su **stato periodico**.
5. Per configurare il numero di sessioni simultanee consentite tra il server che esegue NPS e il SQL Server, digitare un numero in **numero massimo di sessioni simultanee**.
6. Per configurare l'origine dati SQL Server, in **SQL Server registrazione**fare clic su **Configura**. Verrà visualizzata la finestra di dialogo **proprietà di data link** . Nella scheda **connessione** specificare quanto segue: 
    - Per specificare il nome del server in cui è archiviato il database, digitare o selezionare un nome in **selezionare o immettere un nome di server**.
    - Per specificare il metodo di autenticazione con cui accedere al server, fare clic su **Usa sicurezza integrata di Windows NT**. In alternativa, fare clic su **Usa nome utente e password specifici**, quindi digitare le credenziali in **nome utente** e **password**.
    - Per consentire una password vuota, fare clic su **password vuota**.
    - Per archiviare la password, fare clic su **Consenti salvataggio password**.
    - Per specificare il database a cui connettersi nel computer che esegue SQL Server, fare clic **su selezionare il database nel server**, quindi selezionare un nome di database nell'elenco.
7. Per testare la connessione tra NPS e SQL Server, fare clic su **Test connessione**. Fare clic su **OK** per chiudere le **proprietà dei collegamenti dati**.
8. In **azione registrazione non riuscita**selezionare **Abilita registrazione file di testo per il failover** se si desidera che NPS continui con la registrazione di file di testo se la registrazione SQL Server non riesce. 
9. In **azione registrazione non riuscita**selezionare **se la registrazione ha esito negativo, eliminare le richieste di connessione** se si desidera che NPS interrompa l'elaborazione dei messaggi di richiesta di accesso quando i file di log sono pieni o non disponibili per qualche motivo. Se si desidera che NPS continui a elaborare le richieste di connessione in caso di errore di registrazione, non selezionare questa casella di controllo.

## <a name="ping-user-name"></a>Nome utente ping

Alcuni server proxy RADIUS e i server di accesso alla rete inviano periodicamente richieste di autenticazione e accounting, note come richieste ping, per verificare che il server dei criteri di rete sia presente nella rete. Queste richieste ping includono nomi utente fittizi. Quando NPS elabora queste richieste, i registri eventi e accounting vengono riempiti con i record di rifiuto di accesso, rendendo più difficile tenere traccia dei record validi.

Quando si configura una voce del registro di sistema per il **nome utente ping**, NPS corrisponde al valore della voce del registro di sistema rispetto al valore del nome utente in ping richieste da altri server. Una voce del registro di sistema **nome utente ping** specifica il nome utente fittizio (o un modello di nome utente, con variabili, che corrisponde al nome utente fittizio) inviato dai server proxy RADIUS e dai server di accesso alla rete. Quando NPS riceve le richieste ping che corrispondono al valore della voce del registro di sistema **nome utente ping** , NPS rifiuta le richieste di autenticazione senza elaborare la richiesta. NPS non registra le transazioni che coinvolgono il nome utente fittizio in tutti i file di log, semplificando l'interpretazione del registro eventi.

Il **nome utente ping** non è installato per impostazione predefinita. È necessario aggiungere **ping nome utente** al registro di sistema. È possibile aggiungere una voce al registro di sistema utilizzando l'editor del registro di sistema.

>[!CAUTION]
>È possibile che eventuali modifiche non corrette del Registro di sistema danneggino gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

### <a name="to-add-ping-user-name-to-the-registry"></a>Per aggiungere ping nome utente al registro di sistema

Ping nome utente può essere aggiunto alla chiave del registro di sistema seguente come valore stringa da un membro del gruppo Administrators locale:

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nome**:`ping user-name`
- **Tipo**:`REG_SZ`
- **Dati**:  *Nome utente*

>[!TIP]
>Per indicare più di un nome utente per un valore **ping nome utente** , immettere un modello di nome, ad esempio un nome DNS, inclusi i caratteri jolly, nei **dati**.
