---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: Guida alla distribuzione di ADFS in Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13ce514dc5f3f70217a26c898cde6fe24d4967c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847382"
---
# <a name="configure-a-federation-server"></a>Configurare un server federativo

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Dopo l'installazione di Active Directory Federation Services \(ADFS\) servizio ruolo nel computer, si è pronti per configurare questo computer come un server federativo. Eseguire una delle operazioni seguenti:  
  
-   [Configurare il primo server federativo in una nuova server farm federativa](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [Aggiungere un server federativo a una server farm federativa esistente](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>Configurare il primo server federativo in una nuova server farm federativa  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Per configurare il primo server federativo in una nuova server farm federativa tramite configurazione guidata servizi di dominio Active Directory  
  
> [!NOTE]  
> Assicurarsi di disporre delle autorizzazioni di amministratore di dominio o dispone di credenziali di amministratore di dominio disponibile prima di eseguire questa procedura.  
  
1.  Nella pagina **Dashboard** di Server Manager fare clic sul flag **Notifiche** e quindi su **Configurare il servizio federativo nel server**.  
  
    Viene aperta la **Configurazione guidata Servizi di dominio Active Directory** .  
  
2.  Nella **pagina iniziale** selezionare **Crea il primo server di federazione di una server farm di federazione**e quindi fare clic su **Avanti**.  
  
3.  Nel **connessione ad AD DS** , specificare un account con autorizzazioni di amministratore di dominio di Active Directory \(AD\) dominio a cui viene aggiunto questo computer e quindi fare clic su **Next**.  
  
4.  Nella pagina **Impostazione proprietà del servizio** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Importare il file con estensione pfx che contiene il Secure Socket Layer \(SSL\) certificato e la chiave ottenuta in precedenza. In [passaggio 2: Registrare un certificato SSL per ADFS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), aver ottenuto il certificato e viene copiato nel computer che si desidera configurare come server federativo. Per importare il file con estensione pfx tramite la procedura guidata, fare clic su **importare**e quindi passare al percorso del file. Quando viene richiesto, immettere la password per il file con estensione pfx.  
  
    -   Specificare un nome per il servizio federativo. Ad esempio, **fs.contoso.com**. Questo nome deve corrispondere al soggetto o i nomi alternativi del soggetto nel certificato.  
  
    -   Specificare un nome visualizzato per il servizio federativo. Ad esempio, **Contoso Corporation**. Questo nome viene visualizzato agli utenti in Active Directory Federation Services \(ADFS\) sign\-nella pagina.  
  
5.  Nel **impostazione Account del servizio** , specificare un account del servizio. È possibile creare o usare un Account del servizio gestito di gruppo esistente \(gMSA\) o usare un account utente di dominio esistente. Se si seleziona l'opzione per creare un nuovo account gMSA, specificare un nome per il nuovo account. Se si seleziona l'opzione per usare un account di dominio o un gMSA esistente, fare clic su **seleziona** per selezionare un account.  
  
    > [!NOTE]  
    > Il vantaggio di usare un account gMSA è relativo auto\-negoziato funzionalità di aggiornamento password.  
  
    > [!WARNING]  
    > Se si desidera utilizzare un account gestito, è necessario disporre almeno un controller di dominio nell'ambiente in cui è in esecuzione il sistema operativo Windows Server 2012.  
    >   
    > Se l'opzione gMSA è disabilitata e viene visualizzato un messaggio di errore, ad esempio **account del servizio gestiti del gruppo non sono disponibili perché non è stata impostata la chiave radice KDS**, è possibile abilitare gMSA nel dominio tramite l'esecuzione di Windows seguenti Comando di PowerShell in un controller di dominio che esegue Windows Server 2012 o versioni successive, nel dominio di Active Directory: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Tornare quindi alla procedura guidata, fare clic su **Previous**e quindi fare clic su **successiva** a Ri\-immettere il **impostazione Account del servizio** pagina. L'opzione gMSA dovrà ora essere abilitata. È possibile selezionarlo e immettere un nome dell'account gMSA che si desidera utilizzare.  
  
6.  Nel **impostazione Database di configurazione** pagina, specificare un database di configurazione di ADFS e quindi fare clic su **successivo**. È possibile creare un database in questo computer utilizzando Database interno di Windows \(WID\), oppure è possibile specificare il percorso e il nome dell'istanza di Microsoft SQL Server.  
  
    Per altre informazioni, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
    > [!IMPORTANT]  
    > Se si desidera creare una farm AD FS e usare SQL Server per archiviare i dati di configurazione, è possibile usare SQL Server 2008 e versioni successive, incluso SQL Server 2012 e SQL Server 2014.  
  
7.  Nella pagina **Verifica opzioni** verificare le opzioni di configurazione selezionate e quindi fare clic su **Avanti**.  
  
8.  Nel **Pre\-controlli previsti** pagina, verificare che tutti i controlli dei prerequisiti siano stati completati correttamente e quindi fare clic su **configura**.  
  
9. Nel **risultati** pagina, esaminare i risultati e controllare se la configurazione è stata completata correttamente e quindi fare clic su **passaggi successivi necessari per completare la distribuzione del servizio federativo**. Per altre informazioni, vedere [passaggi successivi per completare l'installazione di AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
### <a name="BKMK_3"></a>Per configurare il primo server federativo in una nuova server farm federativa tramite Windows PowerShell  
È possibile creare una nuova server farm federativa utilizzando un account gMSA nuovo o esistente o un account utente di dominio esistente.  
  
-   **Se si desidera creare un nuovo server federativo utilizzando un nuovo account gMSA, eseguire le operazioni seguenti:**  
  
    > [!IMPORTANT]  
    > È necessario disporre delle autorizzazioni di amministratore di dominio per creare il primo server federativo in una nuova server farm federativa.  
  
    1.  Nel computer che si desidera configurare come server federativo, assicurarsi che il certificato SSL necessario sia stato importato nel **Computer locale\\Store My** directory. È possibile verificare se è stato importato il certificato SSL eseguendo il comando seguente nella finestra di comando di Windows PowerShell: `dir Cert:\LocalMachine\My`. Il certificato è indicato dalla relativa identificazione personale nel **Computer locale\\Store My** directory.  
  
    2.  Nel controller di dominio, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente per verificare se nel dominio sia stata creata la chiave radice KDS: `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Se non è stato creato in modo che l'output non visualizzato alcuna informazione, eseguire il comando seguente per creare la chiave: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.  
  
    3.  Nel computer che si desidera configurare come server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > Il `$` segno alla fine del comando precedente è obbligatorio.  
  
        Per ottenere il valore per `<certificate_thumbprint>`, eseguire `dir Cert:\LocalMachine\My`e quindi selezionare l'identificazione personale del certificato SSL. Il valore di `<federation_service_name>` è il nome del servizio federativo, ad esempio **fs.contoso.com**.  
  
        > [!NOTE]  
        > Se non si tratta la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration` parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea una farm database interno di Windows. Se si desidera creare una server farm SQL Server, è necessario disporre di un'istanza di SQL Server già installato e funzionante.  
        >   
        > È possibile usare il comando seguente per creare il primo server federativo in una nuova farm che usa un'istanza di SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"` in cui **< SQL\_Host\_nome >** è il nome del server in cui SQL Server è in esecuzione, e **< SQL\_istanza\_name >** è il nome dell'istanza di SQL Server. Se si usa l'istanza predefinita di SQL Server, usare una **SQLConnectionString** pari a "**Zdroj dat\=< SQL\_Host\_nome >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012.  
  
-   **Se si desidera creare un nuovo server federativo utilizzando un account utente di dominio esistente, eseguire le operazioni seguenti:**  
  
    1.  Nel computer che si desidera configurare come server federativo, assicurarsi che il certificato SSL necessario sia stato importato nel **Computer locale\\Store My** directory. È possibile verificare se è stato importato il certificato SSL eseguendo il comando seguente nella finestra di comando di Windows PowerShell: `dir Cert:\LocalMachine\My`. Il certificato è indicato dalla relativa identificazione personale nel **Computer locale\\Store My** directory.  
  
    2.  Nel computer che si desidera configurare come server federativo, aprire la finestra di comando di Windows PowerShell e quindi eseguire il comando seguente: `$fscred = Get-Credential`. Immettere le credenziali dell'account utente di dominio che si desidera utilizzare per l'account del servizio federativo nel dominio formato\\nome utente.  
  
    3.  Nella stessa finestra di comando di Windows PowerShell eseguire il comando seguente:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        Per ottenere il valore per **< certificato\_identificazione personale >**, eseguire `dir Cert:\LocalMachine\My`e quindi selezionare l'identificazione personale del certificato SSL. Il valore di **< federazione\_service\_name >** è il nome del servizio federativo, ad esempio fs.contoso.com.  
  
        > [!NOTE]  
        > Se non si tratta la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration` parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea una farm database interno di Windows. Se si desidera creare una farm SQL Server, è necessario disporre di istanza di SQL Server già installato e funzionante.  
        >   
        > È possibile usare il comando seguente per creare il primo server federativo in una nuova farm che usa un'istanza di SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` in cui **SQL\_Host\_nome** è il nome del server in cui è SQL Server in esecuzione, e **SQL\_istanza\_nome** è il nome dell'istanza di SQL Server. Se si usa l'istanza predefinita di SQL Server, usare una **SQLConnectionString** pari a "**Zdroj dat\=< SQL\_Host\_nome >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm AD FS e usare SQL Server per archiviare i dati di configurazione, è possibile usare SQL Server 2008 e versioni successive, incluso SQL Server 2012 e SQL Server 2014.  
  
## <a name="BKMK_2"></a>Aggiungere un server federativo a una server farm federativa esistente  
  
> [!IMPORTANT]  
> Assicurarsi che siano state completate [passaggio 3: Installare il servizio ruolo ADFS](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md), prima di iniziare le procedure in questa sezione.  
  
> [!IMPORTANT]  
> Assicurarsi di avere ottenuto un server SSL valido certificato di autenticazione prima di completare questa procedura.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Per aggiungere un server federativo a una server farm federativa esistente tramite configurazione guidata servizi di dominio Active Directory  
  
1.  Nella pagina **Dashboard** di Server Manager fare clic sul flag **Notifiche** e quindi su **Configurare il servizio federativo nel server**.  
  
    Viene aperta la **Configurazione guidata Servizi di dominio Active Directory** .  
  
2.  Nel **benvenuto** pagina, selezionare **aggiungere un server federativo a una server farm federativa**e quindi fare clic su **Next**.  
  
3.  Nel **connessione ad AD DS** , specificare un account con autorizzazioni di amministratore di dominio per il dominio di Active Directory a cui viene aggiunto questo computer e quindi fare clic su **successivo**.  
  
4.  Nel **impostazione Farm** pagina, specificare il nome del server federativo primario in una farm che Usa database interno di Windows oppure specificare il nome host del database e il nome dell'istanza del database di una server farm federativa esistente che utilizza SQL Server.  
  
    > [!WARNING]  
    > In Windows Server® 2012 R2, è disponibile una soluzione per specificare l'istanza predefinita di SQL Server. La soluzione alternativa consiste nel non utilizzare l'interfaccia utente. Usare invece la procedura descritta in [per configurare il primo server federativo in una nuova server farm federativa tramite Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).  
  
    > [!IMPORTANT]  
    > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012.  
  
5.  Nel **specifica il certificato SSL** pagina, importare il file con estensione pfx che contiene il certificato SSL e la chiave che sono state ottenute in precedenza. Questo è certificato di autenticazione del servizio obbligatorio. In [passaggio 2: Registrare un certificato SSL per ADFS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), aver ottenuto il certificato e viene copiato nel computer in cui si desidera configurare come server federativo. Per importare il file con estensione pfx tramite la procedura guidata, fare clic su **importare** e passare al percorso del file. Quando viene richiesto, immettere la password per il file con estensione pfx.  
  
6.  Nel **impostazione Account del servizio** , specificare lo stesso account di servizio configurato durante la creazione del primo server federativo nella farm. È possibile usare un Account del servizio gestito di gruppo esistente o un account utente di dominio esistente.  
  
    > [!IMPORTANT]  
    > L'account specificato deve essere lo stesso account come account che è stato usato nel server federativo primario nella farm.  
  
7.  Nella pagina **Verifica opzioni** verificare le opzioni di configurazione selezionate e quindi fare clic su **Avanti**.  
  
8.  Nel **Pre\-controlli previsti** pagina, verificare che tutti i controlli dei prerequisiti siano stati completati correttamente e quindi fare clic su **configura**.  
  
9. Nel **risultati** pagina, esaminare i risultati e controllare se la configurazione è stata completata correttamente e quindi fare clic su **passaggi successivi necessari per completare la distribuzione del servizio federativo**. Per altre informazioni, vedere [passaggi successivi per completare l'installazione di AD FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Per aggiungere un server federativo a una server farm federativa esistente tramite Windows PowerShell  
È possibile aggiungere un server federativo a una farm esistente usando un account gMSA esistente o un account utente di dominio esistente.  
  
-   Se si desidera aggiungere un server federativo a una farm con un account gMSA esistente, eseguire le operazioni seguenti:  
  
    1.  Nel computer che si desidera configurare come server federativo, assicurarsi che il certificato SSL necessario sia stato importato nel **Computer locale\\Store My** directory. È possibile verificare se è stato importato il certificato SSL eseguendo il comando seguente nella finestra di comando di Windows PowerShell: `dir Cert:\LocalMachine\My`. Il certificato è indicato dalla relativa identificazione personale nel **Computer locale\\Store My** directory.  
  
    2.  Nel computer che si desidera configurare come server federativo, aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>` è il dominio di Active Directory e il nome dell'account gMSA in tale dominio. `<first_federation_server_hostname>` è il nome host del server federativo primario nella farm esistente.  
  
        È possibile ottenere il valore per `<certificate_thumbprint>` eseguendo `dir Cert:\LocalMachine\My` nel passaggio precedente.  
  
        > [!NOTE]  
        > Se non si tratta la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration` parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea un nodo della farm database interno di Windows. Se si desidera creare un nodo della server farm di computer che eseguono SQL Server, è necessario disporre di istanza di SQL Server già installato e funzionante.  
        >   
        > È possibile usare il comando seguente per aggiungere un server federativo a una farm esistente che usa un'istanza di SQL Server: `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` in cui **SQL\_Host\_nome** è il nome del server in cui è SQL Server in esecuzione, e **SQL\_istanza\_nome** è il nome dell'istanza di SQL Server. Se si usa l'istanza predefinita di SQL Server, usare una **SQLConnectionString** pari a "**Zdroj dat\=< SQL\_Host\_nome >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm AD FS e usare SQL Server per archiviare i dati di configurazione, è possibile usare SQL Server 2008 e versioni successive, incluso SQL Server 2012 e SQL Server 2014.  
  
-   Se si desidera aggiungere un server federativo a una farm con un account utente di dominio esistente, eseguire le operazioni seguenti:  
  
    1.  Nel computer che si desidera configurare come server federativo, aprire la finestra di Windows PowerShellcommand, e quindi eseguire il comando seguente: `$fscred = get-credential`. Immettere le credenziali dell'account utente di dominio che si desidera utilizzare per l'account del servizio federativo nel dominio formato\\nome utente.  
  
    2.  Nel computer che si desidera configurare come server federativo, assicurarsi che il certificato SSL necessario sia stato importato nel **Computer locale\\Store My** directory. È possibile verificare se è stato importato il certificato SSL eseguendo il comando seguente nella finestra di Windows PowerShellcommand: `dir Cert:\LocalMachine\My`. Il certificato è indicato dalla relativa identificazione personale nel **Computer locale\\Store My** directory.  
  
    3.  Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > Se non si tratta la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration` parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea un nodo della farm database interno di Windows. Se si desidera creare un nodo della server farm di computer che eseguono SQL Server, è necessario disporre di istanza di SQL Server già installato e funzionante. È possibile usare il comando seguente per aggiungere un server federativo a una farm esistente usando un'istanza di SQL Server: `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` in cui **SQL\_Host\_nome** è il nome del server in cui l'istanza di SQL Server è in esecuzione, e **SQL\_istanza\_nome** è il nome dell'istanza di SQL Server. Se si usa l'istanza predefinita di SQL Server, usare una **SQLConnectionString** pari a "**Zdroj dat\=< SQL\_Host\_nome >; Integrated Security\=True** ".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm AD FS e usare SQL Server per archiviare i dati di configurazione, è possibile usare SQL Server 2008 e versioni successive, incluso SQL Server 2012 e SQL Server 2014.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una Server Farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

