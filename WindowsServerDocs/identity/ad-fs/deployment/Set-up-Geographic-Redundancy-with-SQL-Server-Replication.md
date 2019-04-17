---
title: Il programma di installazione ridondanza geografica con la replica di SQL Server
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: a9f29c1eb19a8241eac5afb5c87581e6c988c3c8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>Il programma di installazione ridondanza geografica con la replica di SQL Server

>Si applica a: Windows Server 2016, Windows Server 2012 R2

> [!IMPORTANT]  
> Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 o versione successiva.
  
Se si utilizza SQL Server come database di configurazione di ADFS, è possibile impostare geo\ ridondanza per la farm ADFS utilizzando la replica di SQL Server. Geo\ ridondanza replica i dati tra due siti geograficamente distanti in modo che le applicazioni possono passare da un sito a altro. In questo modo, in caso di errore di un sito, è possibile ancora tutti i dati di configurazione disponibili nel secondo sito. Per ulteriori informazioni, vedere la sezione "SQL Server ridondanza geografica""in [Federation Server Farm utilizzando SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Prerequisiti  
Installare e configurare una farm SQL server. Per ulteriori informazioni, vedere [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). La versione iniziale di SQL Server, assicurarsi che il servizio Agente SQL Server sia in esecuzione e impostato per l'avvio automatico.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Creare la seconda \(replica\) SQL Server per la ridondanza di geo\  
  
1.  Installare SQL Server \ (per ulteriori informazioni, vedere [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copiare i file di script file createdb. SQL e SetPermissions.sql risultanti nel server di replica SQL.  
  
2.  Assicurarsi che il servizio Agente SQL Server è in esecuzione e impostato per l'avvio automatico  
  
3.  Eseguire **Export-AdfsDeploymentSQLScript** nel nodo di ADFS primario per creare file createdb. SQL e SetPermissions.sql i file.  Ad esempio:`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql2.png)
  
4.  Copiare gli script per il server secondario.  Aprire lo script file createdb. SQL in **SQL Management Studio** e fare clic su **Execute**.
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql4.png)

5. Aprire lo script SetPermissions.sql in **SQL Management Studio** e fare clic su **Execute**.
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql6.png) 

   

>[!NOTE]
>È inoltre possibile utilizzare il comando seguente dalla riga di comando. 
>
>    `c:\>sqlcmd –i CreateDB.sql`  
>  
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Creare le impostazioni di server di pubblicazione nel Server SQL iniziale  
  
1.  Da SQL Server Management studio, in **replica**, fare clic destro **pubblicazioni locali** e scegli **nuova pubblicazione... **
![ Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql7.png) </br>  

2.  Nella schermata di pubblicazione guidata fare clic su **Avanti**.</br>
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql8.png) </br> 
  
3.  In **distributore** pagina, selezionare il server locale come server di distribuzione, fare clic su **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql9.png) </br>   

4.  Nel **Snapshot** cartella pagina, immettere \\\SQL1\repldata al posto di cartella predefinita. \ (Nota: potrebbe essere necessario creare questo yourself\ condivisione).  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql10.png) </br>   
  
5.  Scegliere **AdfsConfigurationV3** come database di pubblicazione e fare clic su **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql11.png) </br>
  
6.  In **tipo di pubblicazione**, scegli **pubblicazione di tipo Merge** e fare clic su **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql12.png) </br>
  
7.  In **tipi di server di sottoscrizione**, scegli **SQL Server 2008 o versione successiva** e fare clic su **Avanti**.  
 ![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql13.png) </br> 

8.  Nel **articoli** pagina Seleziona **tabelle** nodo per selezionare tutte le tabelle, quindi **controllo SyncProperties** tabella \ (questo non deve essere replicated\)</br>
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql14.png) </br>    
  
9.  Nel **articoli** selezionare **funzioni definite dall'utente** nodo per selezionare tutte le funzioni definite dall'utente e fare clic su **Avanti**..  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql15.png) </br>    

10. Nel **articolo problemi** fare clic sul **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql16.png) </br>   

11. Nel **filtrare le righe di tabella** pagina, fare clic su **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql17.png) </br>   
12. Nel **agente Snapshot** pagina Scegli i valori predefiniti di immediato e 14 giorni, fare clic su **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql18.png) </br>   
Si potrebbe essere necessario creare un account di dominio per l'agente SQL. Utilizzare i passaggi descritti in [accesso SQL configurare per l'account di dominio CONTOSO\\sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) per creare account di accesso SQL per il nuovo utente di Active Directory e assegnare autorizzazioni specifiche.  
  
13. Nel **protezione agente** pagina, fare clic su **le impostazioni di sicurezza** e immettere il username\ e la password di un account di dominio \ (non GMSA\) creato per l'agente SQL e fare clic su **OK**.  Fare clic su **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql19.png) </br>  

14. Nel **azioni procedura guidata** pagina, fare clic su **Avanti**.   
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql20.png) </br>

15. Nel **completare la procedura guidata** pagina, immettere un nome per la pubblicazione e fare clic su **fine**. 
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql21.png) </br>  

16. Dopo aver creata la pubblicazione verrà visualizzato lo stato di successo.  Fare clic su **Chiudi**.
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql22.png) </br>  

17. In SQL Server Management Studio, fare doppio clic su nuova pubblicazione indietro e fare clic su **Avvia monitoraggio replica**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Creare impostazioni di sottoscrizione della replica di SQL Server  
Assicurarsi che creato le impostazioni del server di pubblicazione nel Server SQL iniziale come descritto in precedenza e quindi completare la procedura seguente:  
  
1.  SQL Server, la replica da SQL Server Management studio, in **replica**, fare clic destro **sottoscrizioni locali** e scegli **nuova sottoscrizione... **. ![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql24.png) </br>  

2.  Nel **Creazione guidata nuova sottoscrizione** pagina, fare clic su **Avanti**.
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql25.png) </br>   
  
3.  Nel **pubblicazione** pagina, selezionare il server di pubblicazione dall'elenco a discesa.  Espandere **AdfsConfigurationV3** e selezionare il nome della pubblicazione appena creato e fare clic su **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql26.png) </br> 
  
4.  Nel **posizione agente di Merge** selezionare **Esegui ogni agente nel relativo sottoscrittore \(pull subscriptions\)** \(the default\) e fare clic su **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql27.png) </br> Questa operazione, insieme a tipo di sottoscrizione seguente, determina la logica di risoluzione dei conflitti. \ (Per ulteriori informazioni, vedere [rileva e risolvere i conflitti di replica unire](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5.  Nel **sottoscrittori** selezionare **AdfsConfigurationV3** del database di sottoscrizione e fare clic su **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql28.png) </br> 
  
6.  Nel **protezione agente di Merge** pagina, fare clic su **... ** e immettere il nome utente e password di un account di dominio \ (non GMSA\) creato per l'agente SQL utilizzando la casella sui puntini di sospensione e fare clic su **Avanti**.
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql30.png) </br> 
  
7.  In **pianificazione della sincronizzazione**, scegli **eseguire continuamente** e fare clic su **Avanti**. 
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql31.png) </br> 
 
8.  In **inizializzare le sottoscrizioni**, fare clic su **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql32.png) </br> 
  
9.  In **tipo di sottoscrizione**, scegli **Client** e fare clic su **Avanti**.  
  
    Implicazioni di questa sono documentate [qui](https://technet.microsoft.com/library/ms151191.aspx) e [qui](https://technet.microsoft.com/library/ms151170.aspx).  In pratica, dobbiamo tenere la risoluzione dei conflitti "primo su prevale" semplice e non è necessario ripubblicare per altri server di sottoscrizione.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql33.png) </br>
   
10. Nel **azioni procedura guidata** pagina, assicurarsi **creare la sottoscrizione** sia selezionata e fare clic su **Avanti**. 
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql34.png) </br>

11. Nel **completare la procedura guidata** pagina, fare clic su **fine**. 
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql35.png) </br>

12. Al termine il processo di creazione della sottoscrizione, dovresti vedere esito positivo. Fare clic su **Chiudi**. 
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Verificare che il processo di inizializzazione e la replica  
  
1.  In SQL server primario, fare clic con il **replica** nodo e fare clic su **Avvia monitoraggio replica**.  
  
2.  In **Monitoraggio replica**, fare clic sulla pubblicazione.  
  
3.  Nel **tutte le sottoscrizioni** scheda, fare clic destro e **Visualizza dettagli**.  
  
    Dovresti riuscire a vedere numero di voci in **azioni** per la replica iniziale.  
  
4.  Inoltre, è possibile esaminare nel **SQL Server Agent\\Jobs** nodo per visualizzare il job\(s\) pianificata per eseguire le operazioni della publication\/sottoscrizione.  Solo i processi locali vengono visualizzati, pertanto assicurati di controllare il server di pubblicazione e server di sottoscrizione per la risoluzione dei problemi.  Fare clic tenendo premuto con un processo e selezionare **Visualizza cronologia** per visualizzare la cronologia di esecuzione e i risultati.  
  
## <a name="sqlagent"></a>Configurare l'accesso SQL per l'account di dominio CONTOSO\\sqlagent  
  
1.  Creare un nuovo account di accesso sulle primaria e replica SQL Server denominata CONTOSO\\sqlagent \ (il nome del nuovo utente di dominio creato e configurato nel **protezione agente** pagina nelle procedure precedenti. \)  
  
2.  In SQL Server, fare clic con l'account di accesso è stato creato e selezionare proprietà, quindi il **Mapping utente** scheda, questo account di accesso per il mapping **AdfsConfiguration** e **AdfsArtifact** database con ruoli db\_genevaservice e pubblici. Anche eseguire il mapping di questo account di accesso al database di distribuzione e aggiungere il ruolo di db\_owner per la distribuzione e adfsconfiguration tabelle.  Eseguire questa operazione come applicabile a SQL server primario e di replica. Per ulteriori informazioni, vedere [modello di protezione dell'agente di replica](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Assegna la lettura di account di dominio corrispondenti e delle autorizzazioni di scrittura nella condivisione configurata come server di distribuzione.  Assicurarsi che l'impostazione di lettura e scrittura sia le autorizzazioni di condivisione e le autorizzazioni del file locale.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurare ADFS node\(s\) per puntare alla farm replica SQL Server  
Ora che configuri la ridondanza geografica, nodi della farm di ADFS possono essere configurati per puntare della farm di SQL Server di replica utilizzando standard AD FS "funzioni di join" farm, tramite l'interfaccia utente di AD FS configurazione guidata oppure utilizzando Windows PowerShell.  
  
Se si utilizza l'interfaccia utente di AD FS configurazione guidata, selezionare **aggiunge un server federativo a una server farm federativa**. **Non** selezionare **creare il primo server federativo in una server farm federativa**.  
  
Se si utilizza Windows PowerShell, eseguire **Add-AdfsFarmNode**. **Non** eseguire **Install\-AdfsFarm**.  
  
Quando richiesto, fornire il nome host e l'istanza di SQL Server, la replica **non** iniziale SQL server.  
