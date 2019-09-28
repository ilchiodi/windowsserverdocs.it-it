---
ms.assetid: 434fd617-373a-405e-bae4-da324ea83efc
title: Guida alla distribuzione di ADFS in Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c549b52f697db889bf638470aca03f02e1323ed5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359734"
---
# <a name="configure-a-federation-server"></a>Configurare un server federativo

Dopo aver installato il servizio \(ruolo\) Active Directory Federation Services ad FS nel computer, si è pronti per configurare il computer in modo che diventi un server federativo. Eseguire una delle operazioni seguenti:  
  
-   [Configurare il primo server federativo in una nuova Federazione server farm](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_1)  
  
-   [Aggiungere un server federativo a un server farm di federazione esistente](assetId:///e340cf8f-acf3-4cba-8135-a9353b85e714#BKMK_2)  
  
## <a name="BKMK_1"></a>Configurare il primo server federativo in una nuova Federazione server farm  
  
### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Per configurare il primo server federativo in una nuova server farm di federazione utilizzando la configurazione guidata Active Directory Servizio federativo  
  
> [!NOTE]  
> Prima di eseguire questa procedura, assicurarsi di disporre delle autorizzazioni di amministratore di dominio o di disporre delle credenziali di amministratore di dominio.  
  
1.  Nella pagina **Dashboard** di Server Manager fare clic sul flag **Notifiche** e quindi su **Configurare il servizio federativo nel server**.  
  
    Viene aperta la **Configurazione guidata Servizi di dominio Active Directory** .  
  
2.  Nella **pagina iniziale** selezionare **Crea il primo server di federazione di una server farm di federazione**e quindi fare clic su **Avanti**.  
  
3.  Nella pagina **connessione a servizi di dominio Active Directory** specificare un account utilizzando le autorizzazioni di amministratore di dominio \(per\) il Active Directory dominio di Active Directory a cui viene aggiunto il computer e quindi fare clic su **Avanti**.  
  
4.  Nella pagina **Impostazione proprietà del servizio** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:  
  
    -   Importare il file con estensione PFX contenente il certificato SSL \(\) Secure Socket Layer e la chiave ottenuta in precedenza. Nel [passaggio 2: Registrare un certificato SSL per ad FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), il certificato è stato ottenuto e copiato nel computer che si desidera configurare come server federativo. Per importare il file con estensione pfx tramite la procedura guidata, fare clic su **Importa**, quindi selezionare il percorso del file. Quando richiesto, immettere la password per il file con estensione pfx.  
  
    -   Consente di specificare un nome per il servizio federativo. Ad esempio, **FS.contoso.com**. Questo nome deve corrispondere a uno dei nomi alternativi del soggetto o del soggetto nel certificato.  
  
    -   Consente di specificare un nome visualizzato per il servizio federativo. Ad esempio, **Contoso Corporation**. Questo nome viene visualizzato dagli utenti nella \(Active Directory Federation Services\) ad FS\-pagina di accesso.  
  
5.  Nella pagina **Specifica account del servizio** specificare un account del servizio. È possibile creare o usare un account \(del servizio gestito del gruppo esistente gMSA\) o usare un account utente di dominio esistente. Se si seleziona l'opzione per creare un nuovo account gMSA, specificare un nome per il nuovo account. Se si seleziona l'opzione per l'utilizzo di un account di dominio o di gMSA esistente, fare clic su **Seleziona** per selezionare un account.  
  
    > [!NOTE]  
    > Il vantaggio dell'uso di un account gMSA è la\-funzionalità di aggiornamento automatico delle password negoziata.  
  
    > [!WARNING]  
    > Se si vuole usare un account gMSA, è necessario disporre di almeno un controller di dominio nell'ambiente in cui è in esecuzione il sistema operativo Windows Server 2012.  
    >   
    > Se l'opzione gMSA è disabilitata e viene visualizzato un messaggio di errore, ad esempio **gli account del servizio gestito del gruppo non sono disponibili perché la chiave radice KDS non è stata impostata**, è possibile abilitare gMSA nel dominio eseguendo il comando di Windows PowerShell seguente in un dominio controller, che esegue Windows Server 2012 o versione successiva, nel dominio Active Directory: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Tornare quindi alla procedura guidata, fare clic su **precedente**, quindi fare clic su\- **Avanti** per immettere nuovamente la pagina **Specifica account del servizio** . L'opzione gMSA dovrebbe ora essere abilitata. È possibile selezionarlo e immettere un nome di account gMSA che si vuole usare.  
  
6.  Nella pagina **Specifica database di configurazione** specificare un database di configurazione ad FS, quindi fare clic su **Avanti**. È possibile creare un database in questo computer utilizzando database \(interno di Windows wid\)oppure è possibile specificare il percorso e il nome dell'istanza di Microsoft SQL Server.  
  
    Per altre informazioni, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
    > [!IMPORTANT]  
    > Se si desidera creare una farm AD FS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni più recenti, tra cui SQL Server 2012 e SQL Server 2014.  
  
7.  Nella pagina **Verifica opzioni** verificare le opzioni di configurazione selezionate e quindi fare clic su **Avanti**.  
  
8.  Nella pagina **controlli\-dei prerequisiti** verificare che tutti i controlli dei prerequisiti siano stati completati correttamente e quindi fare clic su **Configura**.  
  
9. Nella pagina **risultati** esaminare i risultati e verificare se la configurazione è stata completata correttamente e quindi fare clic su **passaggi successivi necessari per completare la distribuzione del servizio federativo**. Per ulteriori informazioni, vedere [passaggi successivi per il completamento dell'installazione di ad FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
### <a name="BKMK_3"></a>Per configurare il primo server federativo in una nuova Federazione server farm tramite Windows PowerShell  
È possibile creare un nuovo server farm della Federazione usando un account gMSA nuovo o esistente o un account utente di dominio esistente.  
  
-   **Se si desidera creare un nuovo server federativo utilizzando un nuovo account gMSA, eseguire le operazioni seguenti:**  
  
    > [!IMPORTANT]  
    > È necessario disporre delle autorizzazioni di amministratore di dominio per creare il primo server federativo in una nuova server farm federativa.  
  
    1.  Nel computer che si desidera configurare come server federativo, verificare che il certificato SSL richiesto sia stato importato nella directory **Archivio del computer\\locale** . È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra di comando di Windows `dir Cert:\LocalMachine\My`PowerShell:. Il certificato è elencato in base all'identificazione personale nella directory del **\\computer locale archivio** .  
  
    2.  Nel controller di dominio aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente per verificare se la chiave radice KDS è stata creata nel dominio: `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Se non è stato creato in modo che nell'output non vengano visualizzate informazioni, eseguire il comando seguente per creare la chiave `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`:.  
  
    3.  Nel computer che si desidera configurare come server federativo aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$  
        ```  
  
        > [!WARNING]  
        > Il `$` segno alla fine del comando precedente è obbligatorio.  
  
        Per ottenere il valore per `<certificate_thumbprint>`, eseguire `dir Cert:\LocalMachine\My`, quindi selezionare l'identificazione personale del certificato SSL. Il valore di `<federation_service_name>` è il nome del servizio federativo, ad esempio **FS.contoso.com**.  
  
        > [!NOTE]  
        > Se non è la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration` parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea una farm WID. Se si desidera creare un SQL Server server farm, è necessario disporre di un'istanza di SQL Server già installato e operativo.  
        >   
        > È possibile usare il comando seguente per creare il primo server federativo in una nuova farm che usa un'istanza di SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"` dove **< nome\_host\_SQL >** è il nome del server in cui è in esecuzione SQL Server e **< nome\_istanza\_SQL >** è il nome dell'istanza di SQL Server. Se si utilizza l'istanza predefinita di SQL Server, utilizzare un **valore sqlConnectionString** "**Data Source\=< nome host\_\_SQL >; Integrated Security\=true**".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012.  
  
-   **Se si desidera creare un nuovo server federativo utilizzando un account utente di dominio esistente, effettuare le seguenti operazioni:**  
  
    1.  Nel computer che si desidera configurare come server federativo, verificare che il certificato SSL richiesto sia stato importato nella directory **Archivio del computer\\locale** . È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra di comando di Windows `dir Cert:\LocalMachine\My`PowerShell:. Il certificato è elencato in base all'identificazione personale nella directory del **\\computer locale archivio** .  
  
    2.  Nel computer che si desidera configurare come server federativo aprire la finestra di comando di Windows PowerShell, quindi eseguire il comando seguente: `$fscred = Get-Credential`. Immettere le credenziali dell'account utente di dominio che si desidera utilizzare per l'account del servizio federativo nel formato\\dominio nome utente.  
  
    3.  Nella stessa finestra di comando di Windows PowerShell eseguire il comando seguente:  
  
        ```  
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred  
        ```  
  
        Per ottenere il valore per **< identificazione\_personale del certificato >** , eseguire `dir Cert:\LocalMachine\My`, quindi selezionare l'identificazione personale del certificato SSL. Il valore di **< nome\_servizio\_federativo >** è il nome del servizio federativo, ad esempio FS.contoso.com.  
  
        > [!NOTE]  
        > Se non è la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration` parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea una farm WID. Se si desidera creare una farm di SQL Server, è necessario disporre dell'istanza di SQL Server già installata e operativa.  
        >   
        > È possibile usare il comando seguente per creare il primo server federativo in una nuova farm che usa un'istanza di SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` dove **SQL\_host\_Name** è il nome del server in cui è in esecuzione SQL Server e **SQL nomeistanza\_è il nome dell'istanza di SQL Server. \_** Se si utilizza l'istanza predefinita di SQL Server, utilizzare un **valore sqlConnectionString** "**Data Source\=< nome host\_\_SQL >; Integrated Security\=true**".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm AD FS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni più recenti, tra cui SQL Server 2012 e SQL Server 2014.  
  
## <a name="BKMK_2"></a>Aggiungere un server federativo a un server farm di federazione esistente  
  
> [!IMPORTANT]  
> Assicurarsi di aver completato [il passaggio 3: Installare il servizio](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md)ruolo ad FS prima di avviare una delle procedure descritte in questa sezione.  
  
> [!IMPORTANT]  
> Assicurarsi di aver ottenuto un certificato di autenticazione server SSL valido prima di completare questa procedura.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Per aggiungere un server federativo a una server farm federativa esistente tramite la configurazione guidata Active Directory Servizio federativo  
  
1.  Nella pagina **Dashboard** di Server Manager fare clic sul flag **Notifiche** e quindi su **Configurare il servizio federativo nel server**.  
  
    Viene aperta la **Configurazione guidata Servizi di dominio Active Directory** .  
  
2.  Nella pagina **iniziale** selezionare **Aggiungi un server federativo a un server farm di Federazione**, quindi fare clic su **Avanti**.  
  
3.  Nella pagina **connessione a servizi di** dominio Active Directory specificare un account utilizzando le autorizzazioni di amministratore di dominio per il dominio di Active Directory a cui viene aggiunto il computer e quindi fare clic su **Avanti**.  
  
4.  Nella pagina **Specifica farm specificare** il nome del server federativo primario in una farm che utilizza wid o specificare il nome host del database e il nome dell'istanza di database di un server farm di federazione esistente che utilizza SQL Server.  
  
    > [!WARNING]  
    > In Windows Server® 2012 R2 esiste una soluzione alternativa per specificare l'istanza predefinita di SQL Server. La soluzione alternativa consiste nell'evitare di utilizzare l'interfaccia utente. Usare invece i passaggi descritti in [per configurare il primo server federativo in una nuova federazione server farm tramite Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).  
  
    > [!IMPORTANT]  
    > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012.  
  
5.  Nella pagina **specificare il certificato SSL** importare il file con estensione PFX contenente il certificato e la chiave SSL ottenuti in precedenza. Questo è certificato di autenticazione del servizio obbligatorio. Nel [passaggio 2: Registrare un certificato SSL per ad FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), il certificato è stato ottenuto e copiato nel computer che si desidera configurare come server federativo. Per importare il file con estensione pfx tramite la procedura guidata, fare clic su **Importa** e selezionare il percorso del file. Quando richiesto, immettere la password per il file con estensione pfx.  
  
6.  Nella pagina **Specifica account del servizio** specificare lo stesso account di servizio configurato durante la creazione del primo server federativo nella farm. È possibile utilizzare un account del servizio gestito del gruppo esistente o un account utente di dominio esistente.  
  
    > [!IMPORTANT]  
    > L'account specificato deve essere lo stesso account usato nel server federativo primario di questa farm...  
  
7.  Nella pagina **Verifica opzioni** verificare le opzioni di configurazione selezionate e quindi fare clic su **Avanti**.  
  
8.  Nella pagina **controlli\-dei prerequisiti** verificare che tutti i controlli dei prerequisiti siano stati completati correttamente e quindi fare clic su **Configura**.  
  
9. Nella pagina **risultati** esaminare i risultati e verificare se la configurazione è stata completata correttamente e quindi fare clic su **passaggi successivi necessari per completare la distribuzione del servizio federativo**. Per ulteriori informazioni, vedere [passaggi successivi per il completamento dell'installazione di ad FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Per aggiungere un server federativo a un server farm di federazione esistente tramite Windows PowerShell  
È possibile aggiungere un server federativo a una farm esistente usando un account gMSA esistente o un account utente di dominio esistente.  
  
-   Se si desidera aggiungere un server federativo a una farm utilizzando un account gMSA esistente, eseguire le operazioni seguenti:  
  
    1.  Nel computer che si desidera configurare come server federativo, verificare che il certificato SSL richiesto sia stato importato nella directory **Archivio del computer\\locale** . È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra di comando di Windows `dir Cert:\LocalMachine\My`PowerShell:. Il certificato è elencato in base all'identificazione personale nella directory del **\\computer locale archivio** .  
  
    2.  Nel computer che si desidera configurare come server federativo aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.  
  
        ```  
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        `<domain>\<GMSA_name>`è il dominio di Active Directory e il nome dell'account gMSA in tale dominio. `<first_federation_server_hostname>`nome host del server federativo primario della farm esistente.  
  
        È possibile ottenere il valore per `<certificate_thumbprint>` `dir Cert:\LocalMachine\My` eseguendo nel passaggio precedente.  
  
        > [!NOTE]  
        > Se non è la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration` parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea un nodo della farm WID. Se si desidera creare un server farm nodo dei computer che eseguono SQL Server, è necessario disporre dell'istanza di SQL Server già installato e operativo.  
        >   
        > È possibile utilizzare il comando seguente per aggiungere un server federativo a una farm esistente che utilizza un'istanza di SQL Server: `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` dove **nome host\_\_SQL** è il nome del server in cui è in esecuzione SQL Server e **SQL nomeistanza\_è il nome dell'istanza di SQL Server. \_** Se si utilizza l'istanza predefinita di SQL Server, utilizzare un **valore sqlConnectionString** "**Data Source\=< nome host\_\_SQL >; Integrated Security\=true**".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm AD FS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni più recenti, tra cui SQL Server 2012 e SQL Server 2014.  
  
-   Se si desidera aggiungere un server federativo a una farm utilizzando un account utente di dominio esistente, effettuare le seguenti operazioni:  
  
    1.  Nel computer che si desidera configurare come server federativo aprire la finestra PowerShellcommand Windows e quindi eseguire il comando seguente: `$fscred = get-credential`. Immettere le credenziali dell'account utente di dominio che si desidera utilizzare per l'account del servizio federativo nel formato\\dominio nome utente.  
  
    2.  Nel computer che si desidera configurare come server federativo, verificare che il certificato SSL richiesto sia stato importato nella directory **Archivio del computer\\locale** . È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra PowerShellcommand di Windows `dir Cert:\LocalMachine\My`:. Il certificato è elencato in base all'identificazione personale nella directory del **\\computer locale archivio** .  
  
    3.  Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.  
  
        ```  
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>  
        ```  
  
        > [!NOTE]  
        > Se non è la prima volta che si esegue questo comando, aggiungere il `OverwriteConfiguration` parametro.  
  
        > [!NOTE]  
        > Il comando precedente crea un nodo della farm WID. Se si desidera creare un server farm nodo dei computer che eseguono SQL Server, è necessario disporre dell'istanza di SQL Server già installato e operativo. È possibile utilizzare il comando seguente per aggiungere un server federativo a una farm esistente utilizzando un'istanza di SQL Server: `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` dove **nome host\_\_SQL** è il nome del server in cui è in esecuzione l'istanza di SQL Server e **Nome\_istanza\_SQL** è il nome dell'istanza di SQL Server. Se si utilizza l'istanza predefinita di SQL Server, utilizzare un **valore sqlConnectionString** "**Data Source\=< nome host\_\_SQL >; Integrated Security\=true**".  
  
        > [!IMPORTANT]  
        > Se si desidera creare una farm AD FS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni più recenti, tra cui SQL Server 2012 e SQL Server 2014.  
  
## <a name="see-also"></a>Vedere anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

