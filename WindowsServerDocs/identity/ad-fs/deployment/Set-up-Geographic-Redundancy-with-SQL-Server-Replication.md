---
title: Configurare la ridondanza geografica con replica di SQL Server
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: active-directory-federation-services
ms.author: billmath
ms.assetId: 7b9f9a4f-888c-4358-bacd-3237661b1935
ms.openlocfilehash: 16cf1a237043aa546d4fc24164045aa9f9a1e6ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359824"
---
# <a name="setup-geographic-redundancy-with-sql-server-replication"></a>Configurare la ridondanza geografica con replica di SQL Server


> [!IMPORTANT]  
> Se si desidera creare una farm AD FS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 o versione successiva.
  
Se si usa SQL Server come database di configurazione AD FS, è possibile configurare la ridondanza geografica\-per la farm di AD FS usando SQL Server replica. La ridondanza geografica\-replica i dati tra due siti geograficamente distanti, in modo che le applicazioni possano passare da un sito a un altro. In questo modo, in caso di errore di un sito, è comunque possibile disporre di tutti i dati di configurazione disponibili nel secondo sito. Per ulteriori informazioni, vedere la sezione relativa alla ridondanza geografica SQL Server nella [server farm federativa con SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Prerequisiti  
Installare e configurare un server farm SQL. Per ulteriori informazioni, vedere [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). Nella SQL Server iniziale verificare che il servizio SQL Server Agent sia in esecuzione e che sia impostato su avvio automatico.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Creare la seconda replica di \(\) SQL Server per la ridondanza geografica\-  
  
1. Installare SQL Server \(per ulteriori informazioni, vedere [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copiare i file di script CreateDB. SQL e sepermissions. SQL risultanti nell'SQL Server di replica.  
  
2. Verificare che SQL Server Agent servizio sia in esecuzione e che sia impostato su avvio automatico  
  
3. Eseguire **Export\-AdfsDeploymentSQLScript** nel nodo Primary ad FS per creare i file CreateDB. SQL e sepermissions. SQL.  Ad esempio:`PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql2.png)
  
4. Copiare gli script nel server secondario.  Aprire lo script CreateDB. SQL in **sql Management Studio** e fare clic su **Esegui**.
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql4.png)

5. Aprire lo script sepermissions. SQL in **sql Management Studio** e fare clic su **Esegui**.
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql6.png) 

   

> [!NOTE]
> Dalla riga di comando è inoltre possibile utilizzare quanto segue. 
> 
>    `c:\>sqlcmd –i CreateDB.sql`  
> 
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
> ## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Creare le impostazioni dell'editore nella SQL Server iniziale  
  
1. In SQL Server Management Studio, in **replica**, fare clic con il pulsante destro del mouse su **pubblicazioni locali** e scegliere **nuova pubblicazione...** 
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql7.png) </br>  

2. Nella schermata Creazione guidata nuova pubblicazione fare clic su **Avanti**.</br>
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql8.png) </br> 
  
3. Nella pagina server di **distribuzione** scegliere server locale come server di distribuzione e quindi fare clic su **Avanti**.  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql9.png) </br>   

4. Nella pagina cartella **snapshot** immettere \\\SQL1\repldata al posto della cartella predefinita. \(Nota: potrebbe essere necessario creare personalmente la condivisione\).  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql10.png) </br>   
  
5. Scegliere **AdfsConfigurationV3** come database di pubblicazione e fare clic su **Avanti**.  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql11.png) </br>
  
6. In **tipo di pubblicazione**scegliere **pubblicazione di tipo merge** e fare clic su **Avanti**.  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql12.png) </br>
  
7. Sui **tipi di Sottoscrittore**, scegliere **SQL Server 2008 o versione successiva** e fare clic su **Avanti**.  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql13.png) </br> 

8. Nella pagina **articoli** selezionare il nodo **tabelle** per selezionare tutte le tabelle, quindi deselezionare una **\-controllare** la tabella SyncProperties \(questa non deve essere replicata\)</br>
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql14.png) </br>    
  
9. Nella pagina **articoli** selezionare nodo **funzioni definite dall'utente** per selezionare tutte le funzioni definite dall'utente e fare clic su **Avanti**.  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql15.png) </br>    

10. Nella pagina **problemi articolo** fare clic su **Avanti**.  
    ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql16.png) </br>   

11. Nella pagina **Filtro righe tabella** fare clic su **Avanti**.  
    ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql17.png) </br>   
12. Nella pagina **agente di snapshot** scegliere impostazioni predefinite immediate e 14 giorni, quindi fare clic su **Avanti**.  
    ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql18.png) </br>   
    Potrebbe essere necessario creare un account di dominio per SQL Agent. Utilizzare la procedura descritta in [configurare l'accesso SQL per l'account di dominio CONTOSO\\SQLAgent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) per creare l'account di accesso SQL per questo nuovo utente di Active Directory e assegnare autorizzazioni specifiche.  
  
13. Nella pagina **sicurezza agente** fare clic su **impostazioni di sicurezza** e immettere il nome utente\/password di un account di dominio \(non un\) gMSA creato per l'agente SQL, quindi fare clic su **OK**.  Fai clic su **Next**.  
    ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql19.png) </br>  

14. Nella pagina **Azioni procedura guidata** fare clic su **Avanti**.   
    ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql20.png) </br>

15. Nella pagina **Completamento procedura guidata** immettere un nome per la pubblicazione, quindi fare clic su **fine**. 
    ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql21.png) </br>  

16. Una volta creata la pubblicazione, verrà visualizzato lo stato di esito positivo.  Fare clic su **Chiudi**.
    ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql22.png) </br>  

17. Tornare in SQL Server Management Studio, fare clic con il pulsante destro del mouse sulla nuova pubblicazione e scegliere **Avvia monitoraggio replica**.  
    ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql23.png) </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Creare le impostazioni di sottoscrizione nella replica SQL Server  
Assicurarsi di aver creato le impostazioni del server di pubblicazione nella SQL Server iniziale come descritto in precedenza e quindi completare la procedura seguente:  
  
1. Nel SQL Server replica, da SQL Server Management Studio, in **replica**, fare clic con il pulsante destro del mouse su **sottoscrizioni locali** e scegliere **nuova sottoscrizione.** ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql24.png) </br>  

2. Nella pagina **creazione guidata nuova sottoscrizione** fare clic su **Avanti**.
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql25.png) </br>   
  
3. Nella pagina **pubblicazione** selezionare il server di pubblicazione dall'elenco a discesa.  Espandere **AdfsConfigurationV3** e selezionare il nome della pubblicazione creata in precedenza e fare clic su **Avanti**.  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql26.png) </br> 
  
4. Nella pagina **percorso agente di merge** selezionare **Esegui ogni agente nel relativo sottoscrittore \(sottoscrizioni pull\)** \(il\) predefinito e fare clic su **Avanti**.  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql27.png) </br> Questo, insieme al tipo di sottoscrizione riportato di seguito, determina la logica di risoluzione dei conflitti. \(per altre informazioni, vedere [rilevare e risolvere conflitti di replica di tipo merge](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5. Nella pagina **sottoscrittori** selezionare **AdfsConfigurationV3** come database del Sottoscrittore e fare clic su **Avanti**.  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql28.png) </br> 
  
6. Nella pagina **sicurezza agente di merge** fare clic su **...** e immettere il nome utente e la password di un account di dominio \(non un\) gMSA creato per l'agente SQL utilizzando la casella puntini di sospensione e quindi fare clic su **Avanti**.
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql30.png) </br> 
  
7. In **pianificazione della sincronizzazione**scegliere **Esegui** in modo continuo e fare clic su **Avanti**. 
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql31.png) </br> 
 
8. In **Inizializzazione sottoscrizioni**fare clic su **Avanti**.  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql32.png) </br> 
  
9. In **tipo di sottoscrizione**scegliere **client** e fare clic su **Avanti**.  
  
   Le implicazioni di questo argomento sono descritte [qui](https://technet.microsoft.com/library/ms151191.aspx) e [qui](https://technet.microsoft.com/library/ms151170.aspx).  In pratica, viene rilevata la semplice risoluzione dei conflitti "First to Publisher WINS" e non è necessario ripubblicarla in altri Sottoscrittori.  
   ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql33.png) </br>
   
10. Nella pagina **Azioni procedura guidata** , verificare che **Crea la sottoscrizione** sia selezionata e fare clic su **Avanti**. 
    ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql34.png) </br>

11. Nella pagina **Completamento procedura guidata** fare clic su **fine**. 
    ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql35.png) </br>

12. Al termine del processo di creazione della sottoscrizione, verrà visualizzato l'esito positivo. Fare clic su **Chiudi**. 
    ![configurare la ridondanza geografica](media/Set-up-Geographic-Redundancy-with-SQL-Server-Replication/sql36.png) </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Verificare il processo di inizializzazione e replica  
  
1.  Sul server SQL primario, fare clic con il pulsante\-destro del mouse sul nodo **replica** e scegliere **Avvia monitoraggio replica**.  
  
2.  In **Monitoraggio replica**fare clic sulla pubblicazione.  
  
3.  Nella scheda **tutte le sottoscrizioni** fare clic con il pulsante destro del mouse e **visualizzare i dettagli**.  
  
    Si dovrebbe essere in grado di visualizzare molte voci in **azioni** per la replica iniziale.  
  
4.  Inoltre, è possibile cercare nel nodo **SQL Server Agent\\Jobs** per visualizzare il processo\(\) pianificato per l'esecuzione delle operazioni della sottoscrizione\/pubblicazione.  Vengono visualizzati solo i processi locali, quindi assicurarsi di controllare nel server di pubblicazione e nel Sottoscrittore la risoluzione dei problemi.  A destra\-fare clic su un processo e selezionare **Visualizza cronologia** per visualizzare la cronologia di esecuzione e i risultati.  
  
## <a name="sqlagent"></a>Configurare l'account di accesso SQL per l'account di dominio CONTOSO\\SQLAgent  
  
1.  Creare un nuovo account di accesso per il SQL Server primario e la replica denominato CONTOSO\\SQLAgent \(il nome del nuovo utente di dominio creato e configurato nella pagina **sicurezza agente** nelle procedure precedenti.\)  
  
2.  In SQL Server, fare clic con il pulsante\-destro del mouse sull'account di accesso creato e selezionare proprietà, quindi nella scheda **mapping utente** eseguire il mapping di questo account di accesso ai database **AdfsConfiguration** e **AdfsArtifact** con i ruoli Public e DB\_genevaservice. Eseguire inoltre il mapping di questo account di accesso al database di distribuzione e aggiungere il ruolo di proprietario\_database per entrambe le tabelle Distribution e AdfsConfiguration.  Eseguire questa operazione come applicabile sia nel server SQL primario che in quello di replica. Per ulteriori informazioni, vedere [Replication Agent Security Model](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Assegnare all'account di dominio corrispondente le autorizzazioni di lettura e scrittura per la condivisione configurata come server di distribuzione.  Assicurarsi di impostare le autorizzazioni di lettura e scrittura sia per le autorizzazioni di condivisione che per il file locale.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurare AD FS nodo\(s\) in modo che punti alla farm di replica SQL Server  
Ora che è stata impostata la ridondanza geografica, è possibile configurare i nodi della farm AD FS in modo che puntino alla farm della replica SQL Server usando le funzionalità della farm standard AD FS "join", dall'interfaccia utente della configurazione guidata AD FS o usando Windows PowerShell.  
  
Se si utilizza l'interfaccia utente della configurazione guidata di AD FS, selezionare **Aggiungi un server federativo a una server farm federativa**. **Non selezionare** **Crea il primo server federativo in una Federazione server farm**.  
  
Se si usa Windows PowerShell, eseguire **Add\-AdfsFarmNode**. **Non eseguire** **Install\-AdfsFarm**.  
  
Quando richiesto, specificare l'host e il nome dell'istanza della replica SQL Server, **non** l'istanza iniziale di SQL Server.  
