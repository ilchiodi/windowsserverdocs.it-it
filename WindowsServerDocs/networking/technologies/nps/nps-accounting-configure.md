---
title: Configurare le funzionalità di accounting del Server dei criteri di rete
description: In questo argomento vengono fornite informazioni sui file di testo e la registrazione per il Server dei criteri di rete in Windows Server 2016 SQL Server.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: dfde2e21-f3d5-41e8-8492-cb3f0d028afb
ms.author: pashort
author: shortpatti
ms.date: 05/25/2018
ms.openlocfilehash: c732a9f42d942ad579468d1dd15d30324d6fea87
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839702"
---
# <a name="configure-network-policy-server-accounting"></a>Configurare le funzionalità di accounting del Server dei criteri di rete

Esistono tre tipi di registrazioni per il Server dei criteri di rete \(NPS\):

- **La registrazione degli eventi**. Utilizzato principalmente per il controllo e risoluzione dei problemi relativi a tentativi di connessione. È possibile configurare la registrazione per ottenere le proprietà dei criteri di rete nella console Criteri di rete eventi dei criteri di rete.

- **Registrazione delle richieste di accounting e autenticazione degli utenti in un file locale**. Utilizzato principalmente per scopi di analisi e la fatturazione di connessione. Inoltre utile come strumento di analisi di sicurezza perché si offre un metodo di verifica dell'attività di un utente malintenzionato dopo un attacco. È possibile configurare la registrazione di file locale usando la configurazione guidata Accounting.

- **Registrazione delle richieste di accounting e autenticazione degli utenti a un database Microsoft SQL Server compatibile con XML**. Utilizzato per consentire a più server dei criteri di rete per avere una sola origine dati. Fornisce inoltre i vantaggi dell'uso di un database relazionale. È possibile configurare la registrazione di SQL Server usando la configurazione guidata Accounting.

## <a name="use-the-accounting-configuration-wizard"></a>Utilizzare la procedura guidata configurazione di Accounting

Usando la procedura guidata configurazione di Accounting, è possibile configurare le impostazioni di accounting quattro seguenti:

- **La registrazione solo SQL**. Con questa impostazione, è possibile configurare un collegamento di dati a un Server SQL che consente a criteri di rete per connettersi a e inviare i dati di accounting in SQL server. Inoltre, la procedura guidata è possibile configurare il database in SQL Server per garantire che il database è compatibile con la registrazione del server NPS SQL.
- **Testo la registrazione solo**. Con questa impostazione, è possibile configurare criteri di rete per registrare i dati di accounting in un file di testo.
- **La registrazione in parallelo**. Con questa impostazione, è possibile configurare il collegamento di dati di SQL Server e il database. È anche possibile configurare la registrazione in file testo in modo che Criteri di rete contemporaneamente registri per il file di testo e il database di SQL Server. 
- **Registrazione di SQL con backup**. Con questa impostazione, è possibile configurare il collegamento di dati di SQL Server e il database. Inoltre, è possibile configurare la registrazione di file di testo che Criteri di rete viene utilizzato se la registrazione di SQL Server non riesce.

Oltre a queste impostazioni, sia la registrazione di SQL Server e del testo consentono di specificare se i criteri di rete continua a elaborare le richieste di connessione se la registrazione ha esito negativo. È possibile specificare questa nel **sezione di azione di errore di registrazione** nelle proprietà di registrazione di file locale, in proprietà registrazione server SQL e quando si esegue la configurazione guidata di Accounting.

### <a name="to-run-the-accounting-configuration-wizard"></a>Per eseguire la configurazione guidata di Accounting

Per eseguire la configurazione guidata di contabilità, completare i passaggi seguenti:

1. Aprire la console Criteri di rete o lo snap-in Criteri di rete Microsoft Management Console (MMC).
2. Nell'albero della console, fare clic su **Accounting**.
3. Nel riquadro dei dettagli, in **Accounting**, fare clic su **configurare Accounting**.

## <a name="configure-nps-log-file-properties"></a>Configurare le proprietà File di Log criteri di rete

È possibile configurare Server criteri di rete (NPS) per eseguire RADIUS Remote Authentication Dial-In User Service () contabilità per le richieste di autenticazione utente, i messaggi di autorizzazione di accesso, i messaggi di rifiuto di accesso, le richieste di accounting e le risposte e periodiche aggiornamenti di stato. È possibile utilizzare questa procedura per configurare i file di log in cui si desidera archiviare i dati di accounting.

Per altre informazioni sull'interpretazione dei file di log, vedere [interpretare NPS Log file in formato Database](https://technet.microsoft.com/library/cc771748.aspx).

Per impedire che i file di log il riempimento del disco rigido, è consigliabile conservarli in una partizione separata dalla partizione di sistema. Di seguito vengono fornite ulteriori informazioni sulla configurazione di accounting di NPS:

- Per inviare i dati di file di log per la raccolta da un altro processo, è possibile configurare criteri di rete per scrivere in una named pipe. Per usare le named pipe, impostare la cartella di file di log \\. \pipe o \\ComputerName\pipe. Il programma server named pipe consente di creare una named pipe chiamata \\.\pipe\iaslog.log per accettare i dati. Nella finestra di dialogo delle proprietà di file locale, in Crea un nuovo file di log, selezionare mai (dimensioni file illimitate) quando si utilizza named pipe.

- La directory di file di log possono essere create usando le variabili di ambiente di sistema (anziché le variabili utente), ad esempio % systemdrive %, % systemroot % e % windir %. Ad esempio, il percorso seguente, usando la variabile % windir % di ambiente, individua il file di log nella directory di sistema in \System32\Logs la sottocartella (vale a dire %windir%\System32\Logs\).

- Cambio di formati di file di log non determina un nuovo log da creare. Se si modificano i formati di file di log, il file che è attivo al momento della modifica contiene una combinazione dei due formati (record all'inizio del log avrà il formato precedente e i record alla fine del log avrà il nuovo formato).

- Se accounting RADIUS ha esito negativo a causa di un disco rigido pieno o altre cause, dei criteri di rete si arresta l'elaborazione delle richieste di connessione, impedendo agli utenti di accedere alle risorse di rete.

- Criteri di rete offre la possibilità di accedere a un database di Microsoft® SQL Server™ in aggiunta, o in sostituzione di registrazione in un file locale.

Appartenenza al gruppo il **Domain Admins** gruppo è il requisito minimo necessario per eseguire questa procedura.


### <a name="to-configure-nps-log-file-properties"></a>Per configurare le proprietà file di log criteri di rete

1. Aprire la console Criteri di rete o lo snap-in Criteri di rete Microsoft Management Console (MMC).
2. Nell'albero della console, fare clic su **Accounting**.
3. Nel riquadro dei dettagli, in **le proprietà del File di registro**, fare clic su **modificare le proprietà File di Log**. Il **le proprietà del File di registro** verrà visualizzata la finestra di dialogo.
4. In **le proprietà del File di registro**via il **impostazioni** nella scheda **Log le informazioni seguenti**, assicurarsi di aver scelto per registrare informazioni sufficienti per raggiungere gli obiettivi di accounting. Ad esempio, se i log necessari eseguire la correlazione della sessione, selezionare tutte le caselle di controllo.
5. Nelle **azione di errore di registrazione**, selezionare **se la registrazione ha esito negativo, ignorare le richieste di connessione** se si desidera che i criteri di rete per arrestare l'elaborazione dei messaggi di richiesta di accesso quando i file di log sono completo o non è disponibile per qualche motivo. Se si desidera continuare a elaborare le richieste di connessione se la registrazione ha esito negativo dei criteri di rete, non selezionare questa casella di controllo.
6. Nel **le proprietà del File di registro** della finestra di dialogo fare clic sul **File di Log** scheda.
7. Nel **File di Log** nella scheda **Directory**, digitare il percorso in cui si desidera archiviare i file di log criteri di rete. Il percorso predefinito è la cartella systemroot\System32\LogFiles.<br>Se non si specifica un'istruzione di percorso completo nel **Directory File di Log**, viene usato il percorso predefinito. Ad esempio, se si digita **NPSLogFile** nelle **Directory File di Log**, il file si trova in % systemroot%\System32\NPSLogFile.
8. Nelle **formato**, fare clic su **DTS conformi**. Se si preferisce, è possibile invece selezionare un formato di file legacy, ad esempio **ODBC \(Legacy\)**  oppure **IAS \(Legacy\)**.<br>**ODBC** e **IAS** i tipi di file legacy contengono un subset delle informazioni che invia i criteri di rete per il database SQL Server. Il **conforme a DTS** formato XML del tipo di file è identico al formato XML dei criteri di rete viene utilizzato per importare dati in un database SQL Server. Pertanto, il **conforme a DTS** formato di file fornisce un trasferimento più efficiente e completato dei dati nel database di SQL Server standard per criteri di rete.
9. Nelle **creare un nuovo file di log**, configurare criteri di rete per avviare nuovi file di log a intervalli specificati, scegliere l'intervallo che si desidera utilizzare:
    - Per volumi elevati di transazioni e attività di registrazione, fare clic su **giornaliero**.
    - Per volumi di transazioni e attività di registrazione minore, fare clic su **settimanale** oppure **mensili**.
    - Per archiviare tutte le transazioni in un file di log, fare clic su **Never \(dimensioni file illimitate\)**.
    - Per limitare le dimensioni di ogni file di log, fare clic su **quando i file di log raggiunge questa dimensione**e quindi digitare una dimensione del file dopo il quale viene creato un nuovo log. La dimensione predefinita è 10 megabyte (MB).
10. Se si desidera eliminare i vecchi file di log per creare spazio su disco per nuovi file di log quando il disco rigido è quasi esaurito la capacità dei criteri di rete, assicurarsi che **quando disco è pieno Elimina vecchi file di registro** sia selezionata. Questa opzione non è disponibile, tuttavia, se il valore di **creare un nuovo file di log** viene **Never \(dimensioni file illimitate\)**. Inoltre, se il file di log meno recente è il file di log corrente, non viene eliminato.

## <a name="configure-nps-sql-server-logging"></a>Configurare la registrazione di Server dei criteri di rete SQL

È possibile usare questa procedura per i dati di accounting RADIUS di log a un database locale o remoto che esegue Microsoft SQL Server.

>[!NOTE]
>NPS formatta i dati di accounting come un documento XML inviato al **report_event** stored procedure nel database di SQL Server che definiscono in Criteri di rete. Per la registrazione per il corretto funzionamento SQL Server, è necessario disporre di una stored procedure denominata **report_event** nel database di SQL Server che può ricevere e analizzare i documenti XML da NPS.

L'appartenenza al gruppo Domain Admins, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-configure-sql-server-logging-in-nps"></a>Per configurare la registrazione in Criteri di rete SQL Server

1. Aprire la console Criteri di rete o lo snap-in Criteri di rete Microsoft Management Console (MMC).
2. Nell'albero della console, fare clic su **Accounting**.
3. Nel riquadro dei dettagli, in **proprietà registrazione in SQL Server**, fare clic su **Modifica proprietà SQL Server registrazione**. Il **proprietà registrazione in SQL Server** verrà visualizzata la finestra di dialogo.
4. Nelle **registrare le informazioni seguenti**, selezionare le informazioni che si desidera registrare: 
    - Per registrare tutte le richieste di accounting, fare clic su **richieste di Accounting**.
    - Per registrare le richieste di autenticazione, fare clic su **richieste di autenticazione**.
    - Per registrare lo stato di accounting periodico, fare clic su **stato di accounting periodico**.
    - Per registrare lo stato periodico, ad esempio richieste di accounting provvisorio, fare clic su **stato periodici**.
5. Per configurare il numero di sessioni simultanee consentite tra il server NPS e SQL Server, digitare un numero **numero massimo di sessioni simultanee**.
6. Per configurare l'origine dati di SQL Server, in **la registrazione di SQL Server**, fare clic su **configura**. Il **proprietà di Data Link** verrà visualizzata la finestra di dialogo. Nel **connessione** scheda, specificare quanto segue: 
    - Per specificare il nome del server in cui è archiviato il database, digitare o selezionare un nome in **selezionare o immettere un nome di server**.
    - Per specificare il metodo di autenticazione con cui si accede al server, fare clic su **utilizza Windows protezione integrata NT**. In alternativa, scegliere **usare un nome utente specifico e una password**e quindi digitare le credenziali in **nome utente** e **Password**.
    - Per consentire una password vuota, fare clic su **passwor**.
    - Per archiviare la password, fare clic su **Consenti salvataggio password**.
    - Per specificare il database a cui connettersi nel computer che esegue SQL Server, fare clic su **selezionare il database nel server**e quindi selezionare un nome di database dall'elenco.
7. Per testare la connessione tra server NPS e SQL Server, fare clic su **Test connessione**. Fare clic su **OK** per chiudere **proprietà di Data Link**.
8. Nelle **azione di errore di registrazione**, selezionare **abilitare la registrazione di file di testo per il failover** se si desidera che i criteri di rete per continuare con la registrazione file text se la registrazione di SQL Server non riesce. 
9. Nelle **azione di errore di registrazione**, selezionare **se la registrazione ha esito negativo, ignorare le richieste di connessione** se si desidera che i criteri di rete per arrestare l'elaborazione dei messaggi di richiesta di accesso quando i file di log sono completo o non è disponibile per qualche motivo. Se si desidera continuare a elaborare le richieste di connessione se la registrazione ha esito negativo dei criteri di rete, non selezionare questa casella di controllo.

## <a name="ping-user-name"></a>Ping del nome utente

Alcuni server proxy RADIUS e server di accesso alla rete inviano periodicamente richieste di autenticazione e accounting (note come richieste di ping) per verificare che i criteri di rete sia presente nella rete. Queste richieste di ping includono i nomi utente fittizia. Quando Criteri di rete elabora le richieste, i registri eventi e l'accounting vengono riempiti con record di rifiuto di accesso, rendendo più difficile tenere traccia dei record valido.

Quando si configura una voce del Registro di sistema per **effettuare il ping del nome utente**, NPS corrisponde il valore della voce del Registro di sistema rispetto al valore di nome utente nelle richieste di ping da altri server. Oggetto **effettuare il ping del nome utente** voce del Registro di sistema specifica il nome utente fittizio (o un nome modello utente, con le variabili, che corrisponde al nome utente fittizio) inviato dal server proxy RADIUS e server di accesso alla rete. Quando Criteri di rete riceve le richieste di ping corrispondenti di **effettuare il ping del nome utente** valore della voce del Registro di sistema, criteri di rete rifiuta le richieste di autenticazione senza l'elaborazione della richiesta. Criteri di rete non vengono registrate le transazioni che interessano il nome utente fittizio in tutti i file di log, che rende più semplice interpretare il registro eventi.

**Effettuare il ping del nome utente** non è installato per impostazione predefinita. È necessario aggiungere **effettuare il ping del nome utente** nel Registro di sistema. È possibile aggiungere una voce nel Registro di sistema utilizzando l'Editor del Registro di sistema.

>[!CAUTION]
>È possibile che eventuali modifiche non corrette del Registro di sistema danneggino gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

### <a name="to-add-ping-user-name-to-the-registry"></a>Per aggiungere il nome utente ping nel Registro di sistema

Ping del nome utente può essere aggiunto alla chiave del Registro di sistema seguente come un valore stringa da un membro del gruppo Administrators locale:

`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\IAS\Parameters`

- **Nome**: `ping user-name`
- **Tipo**: `REG_SZ`
- **Dati**:  *Nome utente*

>[!TIP]
>Per indicare più di un nome utente per un **effettuare il ping del nome utente** di valore, immettere un modello di nome, ad esempio un nome DNS, compresi i caratteri jolly, nelle **dati**.
