---
title: Configurare l'Accounting Server dei criteri di rete
description: Questo argomento fornisce informazioni sui file di testo e la registrazione per Server dei criteri di rete in Windows Server 2016 SQL Server.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 69341e30d90ee1be29c40d835a4f71fe433c11dc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="configure-network-policy-server-accounting"></a>Configurare l'Accounting Server dei criteri di rete

Esistono tre tipi di registrazione per il Server dei criteri di rete \(NPS\):

- **Registrazione degli eventi**. Utilizzato principalmente per il controllo e risoluzione dei problemi di tentativi di connessione. È possibile configurare la registrazione per ottenere le proprietà di server dei criteri di rete nella console di criteri di rete di eventi dei criteri di rete.

- **Registrazione di autenticazione utente e richieste di accounting in un file locale**. Utilizzata principalmente per scopi di fatturazione e di analisi di connessione. Anche utile come strumento di analisi di sicurezza perché fornisce un metodo di verifica dell'attività di un utente malintenzionato dopo un attacco. È possibile configurare la registrazione in file locali mediante la configurazione guidata Accounting.

- **Registrazione delle richieste di accounting e l'autenticazione utente a un database Microsoft SQL Server XML conforme**. Usato per consentire a più server dei criteri di rete di un'origine dati. Fornisce inoltre i vantaggi dell'utilizzo di un database relazionale. È possibile configurare la registrazione SQL Server utilizzando la configurazione guidata Accounting.

## <a name="use-the-accounting-configuration-wizard"></a>Utilizzare la configurazione guidata Accounting

Utilizzando la procedura guidata configurazione di Accounting, è possibile configurare le impostazioni di accounting quattro seguenti:

- **Solo la registrazione SQL**. Con questa impostazione, è possibile configurare un collegamento di dati a un Server SQL che consente di connettersi e inviare i dati di accounting SQL server dei criteri di rete. Inoltre, la procedura guidata è possibile configurare il database in SQL Server per assicurarsi che il database è compatibile con la registrazione di NPS SQL server.
- **Registrazione di testo solo**. Con questa impostazione, è possibile configurare criteri di rete per registrare i dati di accounting in un file di testo.
- **Registrazione parallelo**. Con questa impostazione, è possibile configurare il database e il collegamento dati SQL Server. È inoltre possibile configurare la registrazione in file testo in modo che i criteri di rete registra contemporaneamente il file di testo e il database di SQL Server. 
- **La registrazione SQL con backup**. Con questa impostazione, è possibile configurare il database e il collegamento dati SQL Server. Inoltre, è possibile configurare la registrazione di file di testo che Criteri di rete viene utilizzato in caso di errore di registrazione SQL Server.

Oltre a queste impostazioni, sia la registrazione SQL Server e la registrazione di testo consentono di specificare se criteri di rete continua a elaborare le richieste di connessione, se la registrazione ha esito negativo. È possibile specificare questo nel **registrazione sezione di azione errore** in proprietà registrazione file locale, in proprietà registrazione server SQL e mentre si esegue la configurazione guidata di Accounting.

### <a name="to-run-the-accounting-configuration-wizard"></a>Per eseguire la configurazione guidata Accounting

Per eseguire la configurazione guidata Accounting, completare i passaggi seguenti:

1. Aprire la console dei criteri di rete o lo snap-in Criteri di rete Microsoft Management Console (MMC).
2. Nell'albero della console, fare clic su **Accounting**.
3. Nel riquadro dei dettagli, in **Accounting**, fare clic su **configurare Accounting**.

## <a name="configure-nps-log-file-properties"></a>Configurare le proprietà del File di registro dei criteri di rete

È possibile configurare Server dei criteri di rete (NPS) per eseguire l'accounting RADIUS Remote Authentication Dial-In User Service () per le richieste di autenticazione utente, messaggi, messaggi di rifiuto di accesso, le richieste di accounting e le risposte e aggiornamenti di stato periodici. È possibile utilizzare questa procedura per configurare i file di registro in cui si desidera archiviare i dati di accounting.

Per ulteriori informazioni sull'interpretazione dei file di registro, vedere [interpretare del file di registro del formato di criteri di rete Database](https://technet.microsoft.com/library/cc771748.aspx).

Per evitare i file di log di riempire il disco rigido, è consigliabile mantenerli in una partizione separata dalla partizione di sistema. Di seguito vengono fornite ulteriori informazioni sulla configurazione di accounting dei criteri di rete:

- Per inviare i dati di file di registro per la raccolta da un altro processo, è possibile configurare criteri di rete per scrivere una named pipe. Per l'utilizzo di named pipe, impostare la cartella di file di registro \\.\pipe o \\ComputerName\pipe. Il programma di server named pipe crea una named pipe denominata \\.\pipe\iaslog.log per accettare i dati. Nella casella di finestra di dialogo Proprietà file locale, nel creare un nuovo file di registro, selezionare mai (dimensioni del file illimitato) quando si utilizza named pipe.

- La directory del file di registro può essere creata utilizzando variabili di ambiente di sistema (invece di variabili utente), ad esempio % systemdrive %, % systemroot % e % windir %. Ad esempio, il percorso seguente, usando la variabile % windir % di ambiente, individua il file di registro nella directory di sistema in \System32\Logs la sottocartella (vale a dire % windir%\System32\Logs\).

- Cambio di formati di file di registro non determina un nuovo registro da creare. Se si modificano i formati file di registro, il file che è attivo al momento della modifica conterrà una combinazione dei due formati (record all'inizio del log avranno il formato precedente e record alla fine del log avrà il nuovo formato).

- Se l'accounting RADIUS ha esito negativo a causa di un disco rigido pieno o altre cause, dei criteri di rete arresta l'elaborazione delle richieste di connessione, impedendo agli utenti di accedere alle risorse di rete.

- Criteri di rete offre la possibilità di accedere a un database Microsoft® SQL Server™, invece di o in aggiunta alla registrazione in un file locale.

Appartenenza al gruppo di **Domain Admins** gruppo è il requisito minimo necessario per eseguire questa procedura.


### <a name="to-configure-nps-log-file-properties"></a>Per configurare le proprietà del file di registro dei criteri di rete

1. Aprire la console dei criteri di rete o lo snap-in Criteri di rete Microsoft Management Console (MMC).
2. Nell'albero della console, fare clic su **Accounting**.
3. Nel riquadro dei dettagli, in **proprietà File di registro**, fare clic su **modificare le proprietà di File di registro**. Il **proprietà File di registro** apre la finestra di dialogo.
4. In **proprietà File di registro**via il **impostazioni** scheda **registrare le seguenti informazioni**, assicurarsi che si sceglie di registrare informazioni sufficienti per conseguire gli obiettivi di accounting. Ad esempio, se i log necessarie per svolgere la correlazione della sessione, selezionare le caselle di controllo.
5. In **azione esito negativo registrazione**selezionare **se la registrazione ha esito negativo, ignorare le richieste di connessione** se si desidera che i criteri di rete per interrompere l'elaborazione dei messaggi di richiesta di accesso quando i file di registro sono pieno o non disponibile per qualche motivo. Se si desidera criteri di rete per continuare a elaborare le richieste di connessione, se la registrazione ha esito negativo, non selezionare questa casella di controllo.
6. Nel **proprietà File di registro** la finestra di dialogo, fare clic su di **File di registro** scheda.
7. Nel **File di registro** scheda **Directory**, digitare il percorso in cui si desidera archiviare i file di registro dei criteri di rete. Il percorso predefinito è la cartella systemroot\System32\LogFiles.
    >[!NOTE]
    >Se non si specifica un'istruzione di percorso completo in **Directory del File di registro**, viene utilizzato il percorso predefinito. Ad esempio, se si digita **NPSLogFile** in **Directory del File di registro**, il file si trova in % systemroot%\System32\NPSLogFile.
8. In **formato**, fare clic su **DTS conforme**. Se si preferisce, è possibile invece selezionare un formato di file legacy, ad esempio **ODBC \(Legacy\)** o **IAS \(Legacy\)**.
    >[!NOTE]
    >**ODBC** e **IAS** tipi di file legacy contengono un sottoinsieme delle informazioni dei criteri di rete invia al proprio database di SQL Server. Il **DTS conforme** formato XML del tipo di file è identico al formato XML che utilizza criteri di rete per importare i dati nel database SQL Server. Di conseguenza, il **DTS conforme** formato di file fornisce un trasferimento più efficiente e completo dei dati nel database di SQL Server standard dei criteri di rete.
9. In **creare un nuovo file di registro**, per configurare criteri di rete per avviare i nuovi file di registro a intervalli specificati, scegliere l'intervallo che si desidera utilizzare:
    - Per i volumi di transazioni e attività di registrazione, fare clic su **giornaliera**.
    - Per minore volumi delle transazioni e attività di registrazione, fare clic su **settimanale** o **mensile**.
    - Per archiviare tutte le transazioni in un file di registro, fare clic su **mai \(unlimited file size\)**.
    - Per limitare le dimensioni di ogni file di registro, fare clic su **quando i file di log raggiunge la dimensione**e quindi digitare un file di dimensioni, dopo che viene creato un nuovo registro. La dimensione predefinita è 10 megabyte (MB).
10. Se si desidera eliminare i file di registro obsoleti per creare spazio su disco per nuovi file di log quando il disco rigido è quasi capacità dei criteri di rete, assicurarsi che **quando disco è pieno Elimina vecchi file di registro** sia selezionata. Questa opzione non è disponibile, tuttavia, se il valore di **creare un nuovo file di registro** è **mai \(unlimited file size\)**. Inoltre, se il file di registro meno recente è il file di registro corrente, non viene eliminato.

## <a name="configure-nps-sql-server-logging"></a>Configurare la registrazione di Server dei criteri di rete SQL

È possibile utilizzare questa procedura per i dati di accounting RADIUS log in un database locale o remoto che esegue Microsoft SQL Server.

>[!NOTE]
>Criteri di rete formatta i dati di accounting come un documento XML che invia il **report_event** stored procedure nel database di SQL Server designati in Criteri di rete. Per SQL Server la registrazione per funzionare correttamente, è necessario disporre di una stored procedure denominata **report_event** nel database di SQL Server che può ricevere e analizzare i documenti XML da server dei criteri.

L'appartenenza al gruppo Domain Admins, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-configure-sql-server-logging-in-nps"></a>Per configurare la registrazione in Criteri di rete SQL Server

1. Aprire la console dei criteri di rete o lo snap-in Criteri di rete Microsoft Management Console (MMC).
2. Nell'albero della console, fare clic su **Accounting**.
3. Nel riquadro dei dettagli, in **proprietà registrazione in SQL Server**, fare clic su **Modifica proprietà SQL Server registrazione**. Il **proprietà registrazione in SQL Server** apre la finestra di dialogo.
4. In **registrare le seguenti informazioni**, selezionare le informazioni che vuoi registrare: 
    - Per accedere a tutte le richieste di accounting, fare clic su **le richieste di Accounting**.
    - Registra le richieste di autenticazione, fare clic su **le richieste di autenticazione**.
    - Per registrare lo stato di accounting periodico, fare clic su **stato di accounting periodico**.
    - Per registrare lo stato periodico, ad esempio richieste di accounting interim, fare clic su **stato periodico**.
5. Per configurare il numero di sessioni simultanee consentite tra il server dei criteri di rete e SQL Server, digitare un numero in **numero massimo di sessioni simultanee**.
6. Per configurare l'origine dati SQL Server, in **la registrazione SQL Server**, fare clic su **configura**. Il **proprietà di collegamento dati** apre la finestra di dialogo. Nel **connessione** scheda, specificare quanto segue: 
    - Per specificare il nome del server in cui è archiviato il database, digitare o selezionare un nome in **selezionare o immettere un nome di server**.
    - Per specificare il metodo di autenticazione con cui si accede al server, fare clic su **utilizza Windows protezione integrata NT**. Fare clic su **utilizzare un nome utente e una password**e quindi digitare le credenziali in **nome utente** e **Password**.
    - Per consentire una password vuota, fare clic su **password vuota**.
    - Per archiviare la password, fare clic su **Consenti salvataggio password**.
    - Per specificare il database a cui connettersi nel computer che esegue SQL Server, fare clic su **selezionare il database nel server di**, quindi selezionare un nome di database dall'elenco.
7. Per testare la connessione tra SQL Server e di criteri di rete, fare clic su **Test connessione**. Fare clic su **OK** per chiudere **proprietà di collegamento dati**.
8. In **azione esito negativo registrazione**selezionare **abilitare la registrazione di file di testo per il failover** se si desidera che Criteri di rete per continuare con la registrazione in file testo in caso di errore di registrazione SQL Server. 
9. In **azione esito negativo registrazione**selezionare **se la registrazione ha esito negativo, ignorare le richieste di connessione** se si desidera che i criteri di rete per interrompere l'elaborazione dei messaggi di richiesta di accesso quando i file di registro sono pieno o non disponibile per qualche motivo. Se si desidera criteri di rete per continuare a elaborare le richieste di connessione, se la registrazione ha esito negativo, non selezionare questa casella di controllo.

## <a name="ping-user-name"></a>Nome utente ping

Alcuni server proxy RADIUS e server di accesso di rete invia periodicamente le richieste di autenticazione e accounting (note come richieste di ping) per verificare che il server dei criteri di rete sia presente nella rete. Queste richieste ping includono nomi utente fittizi. Quando Criteri di rete elabora le richieste, i registri eventi e di accounting diventano pieno record di accesso rifiutato, rendendo più difficile tenere traccia dei record validi.

Quando si configura una voce del Registro di sistema per **effettuare il ping del nome utente**, dei criteri di rete corrisponde al valore della voce del Registro di sistema rispetto al valore di nome utente nelle richieste di ping da altri server. Un **effettuare il ping del nome utente** voce del Registro di sistema specifica il nome utente fittizio (o un nome modello utente, le variabili, che corrisponde al nome utente fittizio) inviato dal server proxy RADIUS e server di accesso alla rete. Quando Criteri di rete riceve le richieste di ping che soddisfano il **effettuare il ping del nome utente** valore della voce del Registro di sistema, dei criteri di rete rifiuta le richieste di autenticazione senza l'elaborazione della richiesta. Criteri di rete non registra le transazioni che coinvolgono il nome utente fittizio in qualsiasi file di registro, che rende più facile da interpretare il registro eventi.

**Effettuare il ping del nome utente** non è installato per impostazione predefinita. È necessario aggiungere **effettuare il ping del nome utente** al Registro di sistema. È possibile aggiungere una voce al Registro di sistema utilizzando l'Editor del Registro di sistema.

>[!CAUTION]
>Potrebbe essere eventuali modifiche non corrette del Registro di sistema danneggino gravemente il sistema. Prima di apportare modifiche al Registro di sistema, è consigliabile eseguire il backup dei dati importanti nel computer.

### <a name="to-add-ping-user-name-to-the-registry"></a>Per aggiungere il nome utente ping al Registro di sistema

Il nome utente ping può essere aggiunti alla seguente chiave del Registro di sistema come valore di stringa da un membro del gruppo Administrators locale:

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nome**: `ping user-name`
- **Tipo**: `REG_SZ`
- **Dati**: *nome utente*

>[!TIP]
>Per più di un nome utente per indicare un **effettuare il ping del nome utente** valore, immettere un modello di nome, ad esempio un nome DNS, inclusi i caratteri jolly, in **dati**.
