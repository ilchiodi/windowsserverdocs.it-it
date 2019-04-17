---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: Guida alla distribuzione di Windows Server 2012 R2 AD ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13ce514dc5f3f70217a26c898cde6fe24d4967c6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-federation-server"></a>Configurare un Server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Dopo aver installato il servizio ruolo di Active Directory Federation Services \(AD FS\) nel computer, si è pronti a configurare questo computer come un server federativo. È possibile eseguire una delle operazioni seguenti:  
  
-   [Configurare il primo server federativo in una nuova server farm federativa](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [Aggiungere un server federativo a una server farm federativa esistente](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>Configurare il primo server federativo in una nuova server farm federativa  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Per configurare il primo server federativo in una nuova server farm federativa tramite la configurazione guidata servizi di Active Directory Federation  
  
> [!NOTE]  
> Assicurarsi di disporre delle autorizzazioni di amministratore di dominio o disporre di credenziali di amministratore di dominio disponibili prima di eseguire questa procedura.  
  
1.  In Server Manager **Dashboard** pagina, fare clic su di **notifiche** flag e quindi fare clic su **configurare il servizio federativo nel server di**.  
  
    Il **servizio Configurazione guidata di Active Directory Federation** apre.  
  
2.  Nel **iniziale** selezionare **creare il primo server federativo in una server farm federativa**, quindi fare clic su **Avanti**.  
  
3.  Nel **connettersi a servizi di dominio Active Directory** specificare un account con autorizzazioni di amministratore di dominio per il dominio di Active Directory \(AD\) a cui viene aggiunto questo computer, quindi fare clic su **Avanti**.  
  
4.  Nel **specificare le proprietà del servizio** pagina, eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Importare il file PFX contenente il certificato SSL (Secure Socket Layer) \(SSL\) e la chiave che sono state ottenute in precedenza. In [passaggio 2: registrare un certificato SSL per ADFS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), aver ottenuto il certificato e viene copiata nel computer che si desidera configurare come server federativo. Per importare il file pfx tramite la procedura guidata, fare clic su **importare**, quindi individuare il percorso del file. Quando viene richiesto, immettere la password per il file con estensione pfx.  
  
    -   Fornire un nome per il servizio federativo. Ad esempio, **fs.contoso.com**. Questo nome deve corrispondere a uno dei nomi alternativi del soggetto del certificato o soggetto.  
  
    -   Specificare un nome visualizzato per il servizio federativo. Ad esempio, **Contoso Corporation**. Gli utenti di questo nome viene visualizzano in \(AD FS\) sign\ di Active Directory Federation Services-nella pagina.  
  
5.  Nel **impostazione Account del servizio** specificare un account del servizio. Creare o utilizzare un \(gMSA\) Account del servizio gestito gruppo esistente oppure utilizzare un account utente di dominio esistente. Se si seleziona l'opzione per creare un nuovo account gestito, specificare un nome per il nuovo account. Se si seleziona l'opzione per utilizzare un account esistente o un account di dominio, fare clic su **selezionare** per selezionare un account.  
  
    > [!NOTE]  
    > Il vantaggio dell'uso di un account gestito è la funzionalità di aggiornamento password negoziato automatico.  
  
    > [!WARNING]  
    > Se si desidera utilizzare un account gestito, è necessario disporre di almeno un controller di dominio nell'ambiente in cui è in esecuzione il sistema operativo Windows Server 2012.  
    >   
    > Se l'opzione gMSA è disabilitata e viene visualizzato un messaggio di errore, ad esempio **account del servizio gestito del gruppo non sono disponibili perché non è stata impostata una chiave radice**, è possibile abilitare gestito nel dominio eseguendo il comando seguente di Windows PowerShell in un controller di dominio che esegue Windows Server 2012 o versioni successive, nel dominio Active Directory:`Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Quindi tornare alla procedura guidata, fare clic su **precedente**, quindi fare clic su **Avanti** per nuovamente-immettere il **impostazione Account del servizio** pagina. L'opzione gMSA ora deve essere abilitata. Puoi selezionarla e immettere un nome di account gMSA che si desidera utilizzare.  
  
6.  Nel **impostazione Database di configurazione** pagina, specificare un database di configurazione di ADFS e quindi fare clic su **Avanti**. È possibile creare un database nel computer utilizzando \(WID\) Database interno di Windows, oppure è possibile specificare il percorso e il nome dell'istanza di Microsoft SQL Server.  
  
    Per ulteriori informazioni, vedere [il ruolo del Database di configurazione AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
    > [!IMPORTANT]  
    > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012 e SQL Server 2014.  
  
7.  Nel **verifica opzioni** pagina, verificare le opzioni di configurazione selezionate e quindi fare clic su **Avanti**.  
  
8.  Nel **controlli pre-necessarie** verificare che tutti i controlli dei prerequisiti siano stati completati correttamente e quindi fare clic su **configura**.  
  
9. Nel **risultati** pagina, esaminare i risultati e controllare se la configurazione sia stata completata correttamente e quindi fare clic su **passaggi successivi necessari per completare la distribuzione del servizio federativo**. Per ulteriori informazioni, vedere [passaggi successivi per completare l'installazione di AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
### <a name="BKMK_3"></a>Per configurare il primo server federativo in una nuova server farm federativa tramite Windows PowerShell  
È possibile creare una nuova server farm federativa utilizzando un account gestito nuovo o esistente o un account utente di dominio esistente.  
  
-   **Se si desidera creare un nuovo server federativo tramite un nuovo account gestito, eseguire le operazioni seguenti:**  
  
    > [!IMPORTANT]  
    > È necessario disporre delle autorizzazioni di amministratore di dominio per creare il primo server federativo in una nuova server farm federativa.  
  
    1.  Nel computer che si desidera configurare come server federativo, assicurarsi che il certificato SSL richiesto è stato importato nel **archivio computer\\Miei locale** directory. È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra di comando Windows PowerShell:`dir Cert:\LocalMachine\My`. Il certificato è elencato dalla relativa identificazione personale nel **archivio computer\\Miei locale** directory.  
  
    2.  Nel controller di dominio, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente per verificare se è stata creata una chiave radice del dominio:`Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Se non è stato creato in modo che l'output non verranno visualizzate informazioni, eseguire il comando seguente per creare la chiave:`Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.  
  
    3.  Nel computer che si desidera configurare come server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > Il `$`accesso alla fine del comando precedente è obbligatorio.  
  
        Per ottenere il valore per `<certificate_thumbprint>`eseguire `dir Cert:\LocalMachine\My`, quindi selezionare l'identificazione personale del certificato SSL. Il valore di `<federation_service_name>`è il nome del servizio federativo, ad esempio, **fs.contoso.com**.  
  
        > [!NOTE]  
        > Se questo non è la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration`parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea una farm database interno di Windows. Se si desidera creare una server farm di SQL Server, è necessario disporre di un'istanza di SQL Server già installato e funzionante.  
        >   
        > È possibile utilizzare il comando seguente per creare il primo server federativo in una nuova farm che utilizza un'istanza di SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"`dove **< SQL\_Host\_Name >** è il nome del server in cui è in esecuzione SQL Server, e **< SQL\_instance\_name >** è il nome dell'istanza di SQL Server. Se si utilizza l'istanza predefinita di SQL Server, utilizzare un **SQLConnectionString** valore di "**Source\ dati = < SQL\_Host\_Name >; Integrated Security\ = True**".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012.  
  
-   **Se si desidera creare un nuovo server federativo utilizzando un account utente di dominio esistente, eseguire le operazioni seguenti:**  
  
    1.  Nel computer che si desidera configurare come server federativo, assicurarsi che il certificato SSL richiesto è stato importato nel **archivio computer\\Miei locale** directory. È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra di comando Windows PowerShell:`dir Cert:\LocalMachine\My`. Il certificato è elencato dalla relativa identificazione personale nel **archivio computer\\Miei locale** directory.  
  
    2.  Nel computer che si desidera configurare come server federativo, aprire la finestra di comando di Windows PowerShell e quindi eseguire il comando seguente:`$fscred = Get-Credential`. Immettere le credenziali dell'account utente di dominio che si desidera utilizzare per l'account del servizio federativo nel formato dominio\\Nome utente nome.  
  
    3.  Nella stessa finestra di comando Windows PowerShell, eseguire il comando seguente:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        Per ottenere il valore per **< certificate\_thumbprint >**eseguire `dir Cert:\LocalMachine\My`, quindi selezionare l'identificazione personale del certificato SSL. Il valore di **< federation\_service\_name >** è il nome del servizio federativo, ad esempio, fs.contoso.com.  
  
        > [!NOTE]  
        > Se questo non è la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration`parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea una farm database interno di Windows. Se si desidera creare una farm SQL Server, è necessario disporre l'istanza di SQL Server già installato e funzionante.  
        >   
        > È possibile utilizzare il comando seguente per creare il primo server federativo in una nuova farm che utilizza un'istanza di SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`dove **SQL\_Host\_Name** è il nome del server in cui è in esecuzione SQL Server, e **SQL\_instance\_name** è il nome dell'istanza di SQL Server. Se si utilizza l'istanza predefinita di SQL Server, utilizzare un **SQLConnectionString** valore di "**Source\ dati = < SQL\_Host\_Name >; Integrated Security\ = True**".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012 e SQL Server 2014.  
  
## <a name="BKMK_2"></a>Aggiungere un server federativo a una server farm federativa esistente  
  
> [!IMPORTANT]  
> Assicurarsi che siano state completate [passaggio 3: installare il servizio ruolo ADFS](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md), prima di avviare una delle procedure in questa sezione.  
  
> [!IMPORTANT]  
> Assicurarsi di avere ottenuto un server SSL valido certificato di autenticazione prima di completare questa procedura.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Per aggiungere un server federativo a una server farm federativa esistente tramite la configurazione guidata servizi di Active Directory Federation  
  
1.  In Server Manager **Dashboard** pagina, fare clic su di **notifiche** flag e quindi fare clic su **configurare il servizio federativo nel server di**.  
  
    Il **servizio Configurazione guidata di Active Directory Federation** apre.  
  
2.  Nel **iniziale** pagina, selezionare **aggiunge un server federativo a una server farm federativa**, quindi fare clic su **Avanti**.  
  
3.  Nel **connettersi a servizi di dominio Active Directory** specificare un account con autorizzazioni di amministratore di dominio per il dominio di Active Directory a cui viene aggiunto questo computer, quindi fare clic su **Avanti**.  
  
4.  Nel **specificare Farm** pagina, specificare il nome del server federativo primario in una farm che Usa database interno di Windows o specificare il nome host del database e il nome dell'istanza del database di una server farm federativa esistente che utilizza SQL Server.  
  
    > [!WARNING]  
    > In Windows Server® 2012 R2, non esiste una soluzione alternativa per specificare l'istanza predefinita di SQL Server. La soluzione consiste nel non utilizzare l'interfaccia utente. Utilizzare invece i passaggi descritti in [per configurare il primo server federativo in una nuova server farm federativa tramite Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).  
  
    > [!IMPORTANT]  
    > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012.  
  
5.  Nel **specifica il certificato SSL** pagina, importare il file PFX contenente il certificato SSL e la chiave che sono state ottenute in precedenza. Questo certificato è il certificato di autenticazione servizio obbligatorio. In [passaggio 2: registrare un certificato SSL per ADFS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), aver ottenuto il certificato e viene copiato nel computer che si desidera configurare come server federativo. Per importare il file pfx tramite la procedura guidata, fare clic su **importare** e individuare il percorso del file. Quando viene richiesto, immettere la password per il file con estensione pfx.  
  
6.  Nel **impostazione Account del servizio** specificare lo stesso account di servizio che è stato configurato quando hai creato il primo server federativo nella farm. È possibile utilizzare un Account del servizio gestito di gruppo esistente o un account utente di dominio esistente.  
  
    > [!IMPORTANT]  
    > L'account specificato deve essere lo stesso account come l'account che è stato utilizzato nel server federativo primario della farm.  
  
7.  Nel **verifica opzioni** pagina, verificare le opzioni di configurazione selezionate e quindi fare clic su **Avanti**.  
  
8.  Nel **controlli pre-necessarie** verificare che tutti i controlli dei prerequisiti siano stati completati correttamente e quindi fare clic su **configura**.  
  
9. Nel **risultati** pagina, esaminare i risultati e controllare se la configurazione sia stata completata correttamente e quindi fare clic su **passaggi successivi necessari per completare la distribuzione del servizio federativo**. Per ulteriori informazioni, vedere [passaggi successivi per completare l'installazione di AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Per aggiungere un server federativo a una server farm federativa esistente tramite Windows PowerShell  
È possibile aggiungere un server federativo a una farm esistente usando un account gestito esistente o un account utente di dominio esistente.  
  
-   Se si desidera aggiungere un server federativo a una farm tramite un account gestito esistente, eseguire le operazioni seguenti:  
  
    1.  Nel computer che si desidera configurare come server federativo, assicurarsi che il certificato SSL richiesto è stato importato nel **archivio computer\\Miei locale** directory. È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra di comando Windows PowerShell:`dir Cert:\LocalMachine\My`. Il certificato è elencato dalla relativa identificazione personale nel **archivio computer\\Miei locale** directory.  
  
    2.  Nel computer che si desidera configurare come server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` è il dominio di Active Directory e il nome del tuo account gMSA in tale dominio. `<first_federation_server_hostname>` è il nome host del server federativo primario della farm esistente.  
  
        È possibile ottenere il valore per `<certificate_thumbprint>`eseguendo `dir Cert:\LocalMachine\My`nel passaggio precedente.  
  
        > [!NOTE]  
        > Se questo non è la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration`parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea un nodo della farm database interno di Windows. Se si desidera creare un nodo della farm di server dei computer che esegue SQL Server, è necessario disporre l'istanza di SQL Server già installato e funzionante.  
        >   
        > È possibile utilizzare il comando seguente per aggiungere un server federativo a una farm esistente che utilizza un'istanza di SQL Server: `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`dove **SQL\_Host\_Name** è il nome del server in cui è in esecuzione SQL Server, e **SQL\_instance\_name** è il nome dell'istanza di SQL Server. Se si utilizza l'istanza predefinita di SQL Server, utilizzare un **SQLConnectionString** valore di "**Source\ dati = < SQL\_Host\_Name >; Integrated Security\ = True**".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012 e SQL Server 2014.  
  
-   Se si desidera aggiungere un server federativo a una farm tramite un account utente di dominio esistente, eseguire le operazioni seguenti:  
  
    1.  Nel computer che si desidera configurare come server federativo, aprire la finestra di Windows PowerShellcommand e quindi eseguire il comando seguente:`$fscred = get-credential`. Immettere le credenziali dell'account utente di dominio che si desidera utilizzare per l'account del servizio federativo nel formato dominio\\Nome utente nome.  
  
    2.  Nel computer che si desidera configurare come server federativo, assicurarsi che il certificato SSL richiesto è stato importato nel **archivio computer\\Miei locale** directory. È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra di Windows PowerShellcommand:`dir Cert:\LocalMachine\My`. Il certificato è elencato dalla relativa identificazione personale nel **archivio computer\\Miei locale** directory.  
  
    3.  Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > Se questo non è la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration`parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea un nodo della farm database interno di Windows. Se si desidera creare un nodo della farm di server dei computer che esegue SQL Server, è necessario disporre l'istanza di SQL Server già installato e funzionante. È possibile utilizzare il comando seguente per aggiungere un server federativo a una farm esistente usando un'istanza di SQL Server: `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"`dove **SQL\_Host\_Name** è il nome del server in cui è in esecuzione l'istanza di SQL Server, e **SQL\_instance\_name** è il nome dell'istanza di SQL Server. Se si utilizza l'istanza predefinita di SQL Server, utilizzare un **SQLConnectionString** valore di "**Source\ dati = < SQL\_Host\_Name >; Integrated Security\ = True**".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012 e SQL Server 2014.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una Server Farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

