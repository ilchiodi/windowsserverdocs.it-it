---
title: Il programma di installazione ridondanza geografica con la replica di SQL Server
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: cf3d7513dd02bdb578bffd2f3ef8bdb29d8f983d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192174"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>Il programma di installazione ridondanza geografica con la replica di SQL Server


> [!IMPORTANT]  
> Se si desidera creare una farm AD FS e usare SQL Server per archiviare i dati di configurazione, è possibile usare SQL Server 2008 o versione successiva.
  
Se si usa SQL Server come database di configurazione di ADFS, è possibile configurare geografica\-ridondanza per la farm AD FS usando la replica di SQL Server. Geografica\-la ridondanza dei dati vengono replicati tra due siti geograficamente distanti e realizzati in modo che le applicazioni è possono passare da un sito a altro. In questo modo, in caso di errore di un sito, è comunque possibile tutti i dati di configurazione disponibili nel secondo sito. Per altre informazioni, vedere la sezione "SQL Server ridondanza geografica" nella [Federation Server Farm utilizzando SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Prerequisiti  
Installare e configurare una farm di server SQL. Per altre informazioni, vedere [ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx). Nel primo Server SQL, assicurarsi che il servizio SQL Server Agent sia in esecuzione e configurato per l'avvio automatico.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Creare la seconda \(replica\) SQL Server per geo\-ridondanza  
  
1.  Installare SQL Server \(per altre informazioni, vedere [ https://technet.microsoft.com/evalcenter/hh225126.aspx ](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copiare i file di script createdb. SQL e SetPermissions.sql risultanti sul server di replica SQL.  
  
2.  Verificare che il servizio SQL Server Agent sia in esecuzione e configurato per l'avvio automatico  
  
3.  Eseguire **esportare\-AdfsDeploymentSQLScript** nel nodo AD FS primario per creare file SetPermissions.sql e createdb. SQL.  Ad esempio:`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql2.png)
  
4.  Copiare gli script sul server secondario.  Aprire lo script createdb. SQL nel **SQL Management Studio** e fare clic su **Execute**.
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql4.png)

5. Aprire lo script SetPermissions.sql **SQL Management Studio** e fare clic su **Execute**.
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql6.png) 

   

>[!NOTE]
>È anche possibile usare il comando seguente dalla riga di comando. 
>
>    `c:\>sqlcmd –i CreateDB.sql`  
>  
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Creare impostazioni server di pubblicazione nel Server SQL iniziale  
  
1.  Da SQL Server Management studio, sotto **replica**, fare clic destro **pubblicazioni locali** e scegliere **nuova pubblicazione... ** 
 ![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql7.png) </br>  

2.  Nella schermata Creazione guidata nuova pubblicazione fare clic su **successivo**.</br>
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql8.png) </br> 
  
3.  Sul **distributore** pagina, scegliere il server locale come server di distribuzione e fare clic su **successivo**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql9.png) </br>   

4.  Nel **Snapshot** cartella immettere \\\SQL1\repldata al posto di cartella predefinita. \(NOTA: È possibile creare manualmente questa condivisione\).  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql10.png) </br>   
  
5.  Scegli **AdfsConfigurationV3** come database di pubblicazione e fare clic su **successivo**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql11.png) </br>
  
6.  Sul **tipo di pubblicazione**, scegliere **pubblicazione di tipo Merge** e fare clic su **Next**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql12.png) </br>
  
7.  Sul **tipi di sottoscrittore**, scegliere **SQL Server 2008 o versioni successive** e fare clic su **Next**.  
 ![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql13.png) </br> 

8.  Nel **articoli** pagina Scegli **tabelle** nodo per selezionare tutte le tabelle, quindi **annullamento\-controllare SyncProperties** tabella \(questo non deve essere replicato\)</br>
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql14.png) </br>    
  
9.  Nel **articoli** pagina, selezionare **funzioni definite dall'utente** nodo selezionare tutte le funzioni definite dall'utente, quindi fare clic su **Next**...  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql15.png) </br>    

10. Nel **problemi articoli** pagina fare clic su **successivo**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql16.png) </br>   

11. Nel **filtro righe tabella** pagina, fare clic su **successivo**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql17.png) </br>   
12. Nel **dell'agente Snapshot** pagina, scegliere le impostazioni predefinite di controllo immediato e 14 giorni, fare clic su **successivo**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql18.png) </br>   
Si potrebbe essere necessario creare un account di dominio per SQL agent. Usare la procedura descritta in [account di accesso di SQL configurare per l'account di dominio CONTOSO\\sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) creare account di accesso SQL per il nuovo utente di Active Directory e assegnare autorizzazioni specifiche.  
  
13. Nel **sicurezza agente** pagina, fare clic su **le impostazioni di sicurezza** e immettere il nome utente\/password di un account di dominio \(non un account GMSA\) creato per SQL agent e Fare clic su **OK**.  Fare clic su **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql19.png) </br>  

14. Nel **azioni procedura guidata** pagina, fare clic su **successivo**.   
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql20.png) </br>

15. Nel **completare la procedura guidata** pagina, immettere un nome per la pubblicazione e fare clic su **fine**. 
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql21.png) </br>  

16. Dopo aver creata la pubblicazione si dovrebbe vedere lo stato di esito positivo.  Fare clic su **Chiudi**.
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql22.png) </br>  

17. In SQL Server Management Studio, fare doppio clic la nuova pubblicazione e fare clic su Indietro **Avvia monitoraggio replica**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Creare le impostazioni di sottoscrizione nella replica di SQL Server  
Verificare che sia creata le impostazioni di pubblicazione nel Server SQL iniziale come descritto in precedenza e quindi completare la procedura seguente:  
  
1.  Nella replica di SQL Server, da SQL Server Management studio, sotto **replica**, fare clic destro **sottoscrizioni locali** e scegliere **nuova sottoscrizione...** . ![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql24.png) </br>  

2.  Nel **Creazione guidata nuova sottoscrizione** pagina, fare clic su **successivo**.
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql25.png) </br>   
  
3.  Nel **Publication** pagina, selezionare il server di pubblicazione dall'elenco a discesa.  Espandere **AdfsConfigurationV3** e selezionare il nome della pubblicazione creata in precedenza e fare clic su **successivo**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql26.png) </br> 
  
4.  Nel **posizione dell'agente di Merge** pagina, selezionare **Esegui ogni agente nel relativo sottoscrittore \(sottoscrizioni pull\)**  \(il valore predefinito\) e fare clic su  **Avanti**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql27.png) </br> Questa operazione, insieme a tipo di sottoscrizione che segue, determina la logica di risoluzione dei conflitti. \(Per altre informazioni, vedere [rilevare e risolvere i conflitti di replica di tipo Merge](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5.  Nel **sottoscrittori** pagina, selezionare **AdfsConfigurationV3** come il database del sottoscrittore fare clic su **Next**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql28.png) </br> 
  
6.  Nel **sicurezza agente di Merge** pagina, fare clic su **...**  e immettere il nome utente e password di un account di dominio \(non è un account GMSA\) creati per l'agente SQL tramite la finestra di puntini di sospensione e fare clic su **successivo**.
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql30.png) </br> 
  
7.  Sul **pianificazione della sincronizzazione**, scegliere **esecuzione continua** e fare clic su **Next**. 
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql31.png) </br> 
 
8.  Sul **inizializzazione sottoscrizioni**, fare clic su **successivo**.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql32.png) </br> 
  
9.  Sul **tipo di sottoscrizione**, scegliere **Client** e fare clic su **Next**.  
  
    Implicazioni di questa sono documentate [Ecco](https://technet.microsoft.com/library/ms151191.aspx) e [qui](https://technet.microsoft.com/library/ms151170.aspx).  In pratica, prendiamo la risoluzione dei conflitti "first su prevale" semplice e non è necessario pubblicare nuovamente in altri sottoscrittori.  
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql33.png) </br>
   
10. Nel **azioni procedura guidata** pagina, assicurarsi **creare la sottoscrizione** sia selezionata e fare clic su **Next**. 
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql34.png) </br>

11. Nel **completare la procedura guidata** pagina, fare clic su **fine**. 
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql35.png) </br>

12. Al termine il processo di creazione della sottoscrizione, si dovrebbe essere esito positivo. Fare clic su **Chiudi**. 
![Impostare la ridondanza geografica](media\Set-up-Geographic-Redundancy-with-SQL-Server-Replication\sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Verificare che il processo di inizializzazione e di replica  
  
1.  Nel server SQL primario, con il pulsante destro\-scegliere il **replica** nodo e fare clic su **Avvia monitoraggio replica**.  
  
2.  Nelle **Monitoraggio replica**, fare clic sulla pubblicazione.  
  
3.  Nel **tutte le sottoscrizioni** fai clic con il pulsante destro e **Visualizza dettagli**.  
  
    Dovrebbe essere possibile visualizzare un numero di voci in **azioni** per la replica iniziale.  
  
4.  Inoltre, è possibile cercare nel **SQL Server Agent\\processi** nodo per visualizzare il processo\(s\) pianificato per eseguire le operazioni di pubblicazione\/sottoscrizione.  Solo i processi locali vengono visualizzati, pertanto assicurarsi di controllare nel server di pubblicazione e nel Sottoscrittore per la risoluzione dei problemi.  A destra\-fare clic su un processo e selezionare **Visualizza cronologia** Visualizza cronologia di esecuzione e i risultati.  
  
## <a name="sqlagent"></a>Configurare account di accesso SQL per l'account di dominio CONTOSO\\sqlagent  
  
1.  Creare un nuovo account di accesso nel database primario e replica di SQL Server denominata CONTOSO\\sqlagent \(il nome del nuovo utente del dominio creati e configurati nel **sicurezza agente** pagina nelle procedure precedenti.\)  
  
2.  In SQL Server, con il pulsante destro\-fare clic su account di accesso è stato creato, quindi scegliere Proprietà, quindi sotto il **Mapping utenti** scheda, eseguire il mapping a questo account di accesso **AdfsConfiguration** e **AdfsArtifact**  i database con public e db\_genevaservice ruoli. Anche il mapping di questo account di accesso al database di distribuzione e aggiungere db\_ruolo di proprietario per le tabelle di distribuzione sia adfsconfiguration.  Eseguire questa operazione come applicabile nel server SQL primari e di replica. Per altre informazioni, vedere [Replication Agent Security Model](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Assegnare la lettura di account di dominio corrispondente e le autorizzazioni di scrittura nella condivisione configurata come server di distribuzione.  Assicurarsi che l'impostazione lettura e scrittura delle autorizzazioni, le autorizzazioni di condivisione e le autorizzazioni del file locale.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurare un nodo di AD FS\(s\) in modo da puntare alla farm di replica di SQL Server  
Ora che configuri la ridondanza geografica, nodi della farm AD FS possono essere configurati per puntare alla farm di SQL Server di replica usando le funzionalità farm di ADFS "join" AD standard, sia dall'interfaccia utente guidata configurazione AD FS o mediante Windows PowerShell.  
  
Se si usa l'interfaccia utente di AD FS configurazione guidata, selezionare **aggiungere un server federativo a una server farm federativa**. **NON li** selezionate **creare il primo server federativo in una server farm federativa**.  
  
Se si usa Windows PowerShell, eseguire **Add\-AdfsFarmNode**. **NON li** eseguiti **installare\-AdfsFarm**.  
  
Quando richiesto, specificare il nome host e istanza di SQL Server, la replica **non** iniziale SQL server.  
