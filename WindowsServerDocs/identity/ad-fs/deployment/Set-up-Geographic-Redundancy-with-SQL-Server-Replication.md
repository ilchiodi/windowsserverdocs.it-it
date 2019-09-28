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
  
Se si usa SQL Server come database di configurazione AD FS, è possibile configurare Geo @ no__t-0redundancy per la farm AD FS usando SQL Server replica. Geo @ no__t-0redundancy replica i dati tra due siti geograficamente distanti, in modo che le applicazioni possano passare da un sito a un altro. In questo modo, in caso di errore di un sito, è comunque possibile disporre di tutti i dati di configurazione disponibili nel secondo sito. Per ulteriori informazioni, vedere la sezione relativa alla ridondanza geografica SQL Server nella [server farm federativa con SQL Server](../design/Federation-Server-Farm-Using-SQL-Server.md).  
  
## <a name="prerequisites"></a>Prerequisiti  
Installare e configurare un server farm SQL. Per ulteriori informazioni, vedere [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). Nella SQL Server iniziale verificare che il servizio SQL Server Agent sia in esecuzione e che sia impostato su avvio automatico.  
  
## <a name="create-the-second-replica-sql-server-for-geo-redundancy"></a>Creare il secondo SQL Server \(replica @ no__t-1 per Geo @ no__t-2redundancy  
  
1. Installare SQL Server \(per altre informazioni, vedere [https://technet.microsoft.com/evalcenter/hh225126.aspx](https://technet.microsoft.com/evalcenter/hh225126.aspx). Copiare i file di script CreateDB. SQL e sepermissions. SQL risultanti nell'SQL Server di replica.  
  
2. Verificare che SQL Server Agent servizio sia in esecuzione e che sia impostato su avvio automatico  
  
3. Eseguire **Export @ no__t-1AdfsDeploymentSQLScript** nel nodo ad FS primario per creare i file CreateDB. SQL e sepermissions. SQL.  Ad esempio: `PS:\>Export-AdfsDeploymentSQLScript -DestinationFolder . –ServiceAccountName CONTOSO\gmsa1$`.  
   @no__t 0Set la ridondanza geografica @ no__t-1
  
4. Copiare gli script nel server secondario.  Aprire lo script CreateDB. SQL in **sql Management Studio** e fare clic su **Esegui**.
   @no__t 0Set la ridondanza geografica @ no__t-1

5. Aprire lo script sepermissions. SQL in **sql Management Studio** e fare clic su **Esegui**.
   @no__t 0Set la ridondanza geografica @ no__t-1 

   

> [!NOTE]
> Dalla riga di comando è inoltre possibile utilizzare quanto segue. 
> 
>    `c:\>sqlcmd –i CreateDB.sql`  
> 
>    `c:\>sqlcmd –i SetPermissions.sql` 
> 
> ## <a name="create-publisher-settings-on-the-initial-sql-server"></a>Creare le impostazioni dell'editore nella SQL Server iniziale  
  
1. In SQL Server Management Studio, in **replica**, fare clic con il pulsante destro del mouse su **pubblicazioni locali** e scegliere **nuova pubblicazione...** 
    @ no__t-4Set di ridondanza geografica @ no__t-5 </br>  

2. Nella schermata Creazione guidata nuova pubblicazione fare clic su **Avanti**.</br>
   @no__t 0Set la ridondanza geografica @ no__t-1 </br> 
  
3. Nella pagina server di **distribuzione** scegliere server locale come server di distribuzione e quindi fare clic su **Avanti**.  
   @no__t 0Set la ridondanza geografica @ no__t-1 </br>   

4. Nella pagina cartella **snapshot** immettere \\ \ SQL1\repldata al posto della cartella predefinita. \(NOTA: Potrebbe essere necessario creare manualmente questa condivisione @ no__t-0.  
   @no__t 0Set la ridondanza geografica @ no__t-1 </br>   
  
5. Scegliere **AdfsConfigurationV3** come database di pubblicazione e fare clic su **Avanti**.  
   @no__t 0Set la ridondanza geografica @ no__t-1 </br>
  
6. In **tipo di pubblicazione**scegliere **pubblicazione di tipo merge** e fare clic su **Avanti**.  
   @no__t 0Set la ridondanza geografica @ no__t-1 </br>
  
7. Sui **tipi di Sottoscrittore**, scegliere **SQL Server 2008 o versione successiva** e fare clic su **Avanti**.  
   @no__t 0Set la ridondanza geografica @ no__t-1 </br> 

8. Nella pagina **articoli** selezionare il nodo **tabelle** per selezionare tutte le tabelle, quindi deselezionare una @no__t tabella **no__t-3Check SyncProperties** -4This. non deve essere replicata @ no__t-5</br>
   @no__t 0Set la ridondanza geografica @ no__t-1 </br>    
  
9. Nella pagina **articoli** selezionare nodo **funzioni definite dall'utente** per selezionare tutte le funzioni definite dall'utente e fare clic su **Avanti**.  
   @no__t 0Set la ridondanza geografica @ no__t-1 </br>    

10. Nella pagina **problemi articolo** fare clic su **Avanti**.  
    @no__t 0Set la ridondanza geografica @ no__t-1 </br>   

11. Nella pagina **Filtro righe tabella** fare clic su **Avanti**.  
    @no__t 0Set la ridondanza geografica @ no__t-1 </br>   
12. Nella pagina **agente di snapshot** scegliere impostazioni predefinite immediate e 14 giorni, quindi fare clic su **Avanti**.  
    @no__t 0Set la ridondanza geografica @ no__t-1 </br>   
    Potrebbe essere necessario creare un account di dominio per SQL Agent. Utilizzare la procedura descritta in [configurare l'accesso SQL per l'account di dominio contoso @ no__t-1sqlagent](Set-up-Geographic-Redundancy-with-SQL-Server-Replication.md#sqlagent) per creare un account di accesso SQL per questo nuovo utente di Active Directory e assegnare autorizzazioni specifiche.  
  
13. Nella pagina **sicurezza agente** fare clic su **impostazioni di sicurezza** e immettere il nome utente @ no__t-2password di un account di dominio \(not a gMSA @ no__t-4 creato per l'agente SQL, quindi fare clic su **OK**.  Fare clic su **Avanti**.  
    @no__t 0Set la ridondanza geografica @ no__t-1 </br>  

14. Nella pagina **Azioni procedura guidata** fare clic su **Avanti**.   
    @no__t 0Set la ridondanza geografica @ no__t-1 </br>

15. Nella pagina **Completamento procedura guidata** immettere un nome per la pubblicazione, quindi fare clic su **fine**. 
    @no__t 0Set la ridondanza geografica @ no__t-1 </br>  

16. Una volta creata la pubblicazione, verrà visualizzato lo stato di esito positivo.  Fare clic su **Chiudi**.
    @no__t 0Set la ridondanza geografica @ no__t-1 </br>  

17. Tornare in SQL Server Management Studio, fare clic con il pulsante destro del mouse sulla nuova pubblicazione e scegliere **Avvia monitoraggio replica**.  
    @no__t 0Set la ridondanza geografica @ no__t-1 </br> 
  
## <a name="create-subscription-settings-on-the-replica-sql-server"></a>Creare le impostazioni di sottoscrizione nella replica SQL Server  
Assicurarsi di aver creato le impostazioni del server di pubblicazione nella SQL Server iniziale come descritto in precedenza e quindi completare la procedura seguente:  
  
1. Nel SQL Server replica, da SQL Server Management Studio, in **replica**, fare clic con il pulsante destro del mouse su **sottoscrizioni locali** e scegliere **nuova sottoscrizione.** @no__t 0Set la ridondanza geografica @ no__t-1 </br>  

2. Nella pagina **creazione guidata nuova sottoscrizione** fare clic su **Avanti**.
   @no__t 0Set la ridondanza geografica @ no__t-1 </br>   
  
3. Nella pagina **pubblicazione** selezionare il server di pubblicazione dall'elenco a discesa.  Espandere **AdfsConfigurationV3** e selezionare il nome della pubblicazione creata in precedenza e fare clic su **Avanti**.  
   @no__t 0Set la ridondanza geografica @ no__t-1 </br> 
  
4. Nella pagina **percorso agente di merge** selezionare **Esegui ogni agente nel relativo sottoscrittore \(pull sottoscrizioni @ no__t-3** \(Il impostazione predefinita @ no__t-5 e fare clic su **Avanti**.  
   @no__t 0Set la ridondanza geografica @ no__t-1 </br> Questo, insieme al tipo di sottoscrizione riportato di seguito, determina la logica di risoluzione dei conflitti. \(per altre informazioni, vedere [rilevare e risolvere i conflitti di replica di tipo merge](https://technet.microsoft.com/library/ms151191.aspx). </br>
 
5. Nella pagina **sottoscrittori** selezionare **AdfsConfigurationV3** come database del Sottoscrittore e fare clic su **Avanti**.  
   @no__t 0Set la ridondanza geografica @ no__t-1 </br> 
  
6. Nella pagina **sicurezza agente di merge** fare clic su **...** e immettere il nome utente e la password di un account di dominio \(not gMSA @ no__t-3 creato per l'agente SQL utilizzando la casella puntini di sospensione e quindi fare clic su **Avanti**.
   @no__t 0Set la ridondanza geografica @ no__t-1 </br> 
  
7. In **pianificazione della sincronizzazione**scegliere **Esegui** in modo continuo e fare clic su **Avanti**. 
   @no__t 0Set la ridondanza geografica @ no__t-1 </br> 
 
8. In **Inizializzazione sottoscrizioni**fare clic su **Avanti**.  
   @no__t 0Set la ridondanza geografica @ no__t-1 </br> 
  
9. In **tipo di sottoscrizione**scegliere **client** e fare clic su **Avanti**.  
  
   Le implicazioni di questo argomento sono descritte [qui](https://technet.microsoft.com/library/ms151191.aspx) e [qui](https://technet.microsoft.com/library/ms151170.aspx).  In pratica, viene rilevata la semplice risoluzione dei conflitti "First to Publisher WINS" e non è necessario ripubblicarla in altri Sottoscrittori.  
   @no__t 0Set la ridondanza geografica @ no__t-1 </br>
   
10. Nella pagina **Azioni procedura guidata** , verificare che **Crea la sottoscrizione** sia selezionata e fare clic su **Avanti**. 
    @no__t 0Set la ridondanza geografica @ no__t-1 </br>

11. Nella pagina **Completamento procedura guidata** fare clic su **fine**. 
    @no__t 0Set la ridondanza geografica @ no__t-1 </br>

12. Al termine del processo di creazione della sottoscrizione, verrà visualizzato l'esito positivo. Fare clic su **Chiudi**. 
    @no__t 0Set la ridondanza geografica @ no__t-1 </br>
  
## <a name="verify-the-process-of-initialization-and-replication"></a>Verificare il processo di inizializzazione e replica  
  
1.  Sul server SQL primario, fare clic con il pulsante destro del mouse su no__t-0click nel nodo **replica** e scegliere **Avvia monitoraggio replica**.  
  
2.  In **Monitoraggio replica**fare clic sulla pubblicazione.  
  
3.  Nella scheda **tutte le sottoscrizioni** fare clic con il pulsante destro del mouse e **visualizzare i dettagli**.  
  
    Si dovrebbe essere in grado di visualizzare molte voci in **azioni** per la replica iniziale.  
  
4.  Inoltre, è possibile esaminare il **SQL Server Agent nodo @ no__t-1Jobs** per visualizzare il processo @ no__t-2S @ no__t-3 pianificato per l'esecuzione delle operazioni della pubblicazione @ no__t-4subscription.  Vengono visualizzati solo i processi locali, quindi assicurarsi di controllare nel server di pubblicazione e nel Sottoscrittore la risoluzione dei problemi.  Right @ no__t-0click un processo e selezionare **Visualizza cronologia** per visualizzare la cronologia di esecuzione e i risultati.  
  
## <a name="sqlagent"></a>Configurare l'account di accesso SQL per l'account di dominio CONTOSO @ no__t-1sqlagent  
  
1.  Creare un nuovo account di accesso per il SQL Server primario e quello di replica denominato CONTOSO @ no__t-0sqlagent \(Il nome del nuovo utente di dominio creato e configurato nella pagina **sicurezza agente** nelle procedure precedenti. \)  
  
2.  In SQL Server, fare clic con il pulsante destro del mouse su no__t-0click dell'account di accesso creato e scegliere Proprietà, quindi nella scheda **mapping utente** eseguire il mapping di questo account di accesso ai database **AdfsConfiguration** e **AdfsArtifact** con i ruoli Public e DB @ no__t-4genevaservice. Eseguire inoltre il mapping di questo account di accesso al database di distribuzione e aggiungere il ruolo DB @ no__t-0owner per entrambe le tabelle Distribution e AdfsConfiguration.  Eseguire questa operazione come applicabile sia nel server SQL primario che in quello di replica. Per ulteriori informazioni, vedere [Replication Agent Security Model](https://technet.microsoft.com/library/ms151868.aspx).  
  
3.  Assegnare all'account di dominio corrispondente le autorizzazioni di lettura e scrittura per la condivisione configurata come server di distribuzione.  Assicurarsi di impostare le autorizzazioni di lettura e scrittura sia per le autorizzazioni di condivisione che per il file locale.  
  
## <a name="configure-ad-fs-nodes-to-point-to-the-sql-server-replica-farm"></a>Configurare AD FS nodo @ no__t-0 @ no__t-1 per puntare alla farm di replica SQL Server  
Ora che è stata impostata la ridondanza geografica, è possibile configurare i nodi della farm AD FS in modo che puntino alla farm della replica SQL Server usando le funzionalità della farm standard AD FS "join", dall'interfaccia utente della configurazione guidata AD FS o usando Windows PowerShell.  
  
Se si utilizza l'interfaccia utente della configurazione guidata di AD FS, selezionare **Aggiungi un server federativo a una server farm federativa**. **Non selezionare** **Crea il primo server federativo in una Federazione server farm**.  
  
Se si usa Windows PowerShell, eseguire **Add @ no__t-1AdfsFarmNode**. **Non eseguire** **Install @ no__t-2AdfsFarm**.  
  
Quando richiesto, specificare l'host e il nome dell'istanza della replica SQL Server, **non** l'istanza iniziale di SQL Server.  
