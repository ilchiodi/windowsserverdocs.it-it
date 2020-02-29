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
ms.openlocfilehash: 9636d1a7b67f3f9c7766d6b986cb9d7c194a5c74
ms.sourcegitcommit: 9687d3eb221b89061a48bf1e73fb3b25bee69f9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2020
ms.locfileid: "78169611"
---
# <a name="configure-a-federation-server"></a>Configurare un server federativo

Dopo aver installato il Active Directory Federation Services \(AD FS servizio ruolo\) nel computer, si è pronti per configurare il computer in modo che diventi un server federativo. È possibile eseguire una delle operazioni seguenti:

-   [Configurare il primo server federativo in una nuova Federazione server farm](Configure-a-Federation-Server.md#BKMK_1)

-   [Aggiungere un server federativo a un server farm di federazione esistente](Configure-a-Federation-Server.md#BKMK_2)

## <a name="BKMK_1"></a>Configurare il primo server federativo in una nuova Federazione server farm

### <a name="to-configure-the-first-federation-server-in-a-new-federation-server-farm-by-using-the-active-directory-federation-service-configuration-wizard"></a>Per configurare il primo server federativo in una nuova server farm di federazione utilizzando la configurazione guidata Active Directory Servizio federativo

> [!NOTE]
> Prima di eseguire questa procedura, assicurarsi di disporre delle autorizzazioni di amministratore di dominio o di disporre delle credenziali di amministratore di dominio.

1.  Nella pagina **Dashboard** di Server Manager fare clic sul flag **Notifiche** e quindi su **Configurare il servizio federativo nel server**.

    Viene aperta la **Configurazione guidata Servizi di dominio Active Directory**.

2.  Nella **pagina iniziale** selezionare **Crea il primo server di federazione di una server farm di federazione** e quindi fare clic su **Avanti**.

3.  Nella pagina **connessione a servizi di dominio Active Directory** specificare un account utilizzando le autorizzazioni di amministratore di dominio per il Active Directory \(dominio ad\) a cui viene aggiunto il computer e quindi fare clic su **Avanti**.

4.  Nella pagina **Impostazione proprietà del servizio** eseguire le operazioni seguenti e quindi fare clic su **Avanti**:

    -   Importare il file con estensione PFX contenente il certificato Secure Socket Layer \(SSL\) e la chiave ottenuta in precedenza. In [passaggio 2: registrare un certificato SSL per ad FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), il certificato è stato ottenuto e copiato nel computer che si desidera configurare come server federativo. Per importare il file con estensione pfx tramite la procedura guidata, fare clic su **Importa**, quindi selezionare il percorso del file. Quando richiesto, immettere la password per il file con estensione pfx.

    -   Specificare un nome per il servizio federativo, ad esempio **fs.contoso.com**. Il nome deve corrispondere a uno dei nomi di soggetto o di soggetto alternativo nel certificato.

    -   Specificare un nome visualizzato per il servizio federativo, ad esempio **Contoso Corporation**. Questo nome viene visualizzato dagli utenti nella Active Directory Federation Services \(AD FS\) segno\-nella pagina.

5.  Nella pagina **Impostazione account servizio** specificare un account di servizio. È possibile creare o utilizzare un account del servizio gestito del gruppo esistente \(gMSA\) oppure utilizzare un account utente di dominio esistente. Se si seleziona l'opzione per creare un nuovo account gMSA, specificare un nome per il nuovo account. Se si seleziona l'opzione per l'utilizzo di un account di dominio o di gMSA esistente, fare clic su **Seleziona** per selezionare un account.

    > [!NOTE]
    > Il vantaggio dell'uso di un account gMSA è la funzionalità di aggiornamento automatico delle password negoziata\-.

    > [!WARNING]
    > Se si vuole usare un account gMSA, è necessario disporre di almeno un controller di dominio nell'ambiente in cui è in esecuzione il sistema operativo Windows Server 2012.
    >
    > Se l'opzione gMSA è disabilitata e viene visualizzato un messaggio di errore, ad esempio **gli account del servizio gestito del gruppo non sono disponibili perché la chiave radice KDS non è stata impostata**, è possibile abilitare gMSA nel dominio eseguendo il comando di Windows PowerShell seguente in un controller di dominio che esegue windows Server 2012 o versione successiva nel dominio Active Directory: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Tornare quindi alla procedura guidata, fare clic su **precedente**, quindi fare clic su **avanti** per ri\-immettere la pagina **Specifica account del servizio** . L'opzione gMSA dovrebbe ora essere abilitata. È possibile selezionarlo e immettere un nome di account gMSA che si vuole usare.

6.  Nella pagina **Specifica database di configurazione** specificare un database di configurazione ad FS, quindi fare clic su **Avanti**. È possibile creare un database in questo computer utilizzando database interno di Windows \(WID\)oppure è possibile specificare il percorso e il nome dell'istanza di Microsoft SQL Server.

    Per altre informazioni, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).

    > [!IMPORTANT]
    > Se si desidera creare una farm AD FS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni più recenti, tra cui SQL Server 2012 e SQL Server 2014.

7.  Nella pagina **Verifica opzioni** verificare le opzioni di configurazione selezionate e quindi fare clic su **Avanti**.

8.  Nella pagina **controlli dei prerequisiti Pre\-** verificare che tutti i controlli dei prerequisiti siano stati completati correttamente e quindi fare clic su **Configura**.

9. Nella pagina **risultati** esaminare i risultati e verificare se la configurazione è stata completata correttamente e quindi fare clic su **passaggi successivi necessari per completare la distribuzione del servizio federativo**. Per ulteriori informazioni, vedere [passaggi successivi per il completamento dell'installazione di ad FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Fare clic su **Chiudi** per uscire dalla procedura guidata.

### <a name="BKMK_3"></a>Per configurare il primo server federativo in una nuova Federazione server farm tramite Windows PowerShell
È possibile creare un nuovo server farm della Federazione usando un account gMSA nuovo o esistente o un account utente di dominio esistente.

-   **Se si desidera creare un nuovo server federativo utilizzando un nuovo account gMSA, eseguire le operazioni seguenti:**

    > [!IMPORTANT]
    > È necessario disporre delle autorizzazioni di amministratore di dominio per creare il primo server federativo in una nuova server farm federativa.

    1.  Nel computer che si desidera configurare come server federativo, verificare che il certificato SSL richiesto sia stato importato nel **computer locale\\directory archivio** . È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra di comando di Windows PowerShell: `dir Cert:\LocalMachine\My`. Il certificato è elencato in base all'identificazione personale nel **computer locale\\** directory dell'archivio.

    2.  Nel controller di dominio aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente per verificare se la chiave radice KDS è stata creata nel dominio: `Get-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`. Se non è stato creato in modo che l'output non visualizzi informazioni, eseguire il comando seguente per creare la chiave: `Add-KdsRootKey –EffectiveTime (Get-Date).AddHours(-10)`.

    3.  Nel computer che si desidera configurare come server federativo aprire la finestra di comando di Windows PowerShell ed eseguire il seguente comando:

        ```
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_Name>$
        ```

        > [!WARNING]
        > È necessario il segno di `$` alla fine del comando precedente.

        Per ottenere il valore per `<certificate_thumbprint>`, eseguire `dir Cert:\LocalMachine\My`, quindi selezionare l'identificazione personale del certificato SSL. Il valore di `<federation_service_name>` è il nome del servizio federativo, ad esempio **fs.contoso.com**.

        > [!NOTE]
        > Se non è la prima volta che si esegue questo comando, aggiungere il parametro `OverwriteConfiguration`.

        > [!NOTE]
        > Il comando precedente crea una farm WID. Se si desidera creare un SQL Server server farm, è necessario disporre di un'istanza di SQL Server già installato e operativo.
        >
        > È possibile utilizzare il comando seguente per creare il primo server federativo in una nuova farm in cui viene utilizzata un'istanza di SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name?\<SQL_instance_ name>;Integrated Security=True"` in cui **< nome\_host di sql\_** è il nome del server in cui è in esecuzione > e SQL Server <\_\_**nome** > è il nome dell'istanza di SQL Server. Se si utilizza l'istanza predefinita di SQL Server, utilizzare un valore **sqlConnectionString** "**Data Source\=< SQL\_host\_nome >; Integrated Security\=true**".

        > [!IMPORTANT]
        > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012.

-   **Se si desidera creare un nuovo server federativo utilizzando un account utente di dominio esistente, effettuare le seguenti operazioni:**

    1.  Nel computer che si desidera configurare come server federativo, verificare che il certificato SSL richiesto sia stato importato nel **computer locale\\directory archivio** . È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra di comando di Windows PowerShell: `dir Cert:\LocalMachine\My`. Il certificato è elencato in base all'identificazione personale nel **computer locale\\** directory dell'archivio.

    2.  Nel computer che si desidera configurare come server federativo aprire la finestra di comando di Windows PowerShell, quindi eseguire il comando seguente: `$fscred = Get-Credential`. Immettere le credenziali dell'account utente di dominio che si desidera utilizzare per l'account del servizio federativo nel formato dominio\\nome utente.

    3.  Nella stessa finestra di comando di Windows PowerShell eseguire il seguente comando:

        ```
        Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscred
        ```

        Per ottenere il valore per **< certificato\_identificazione personale >** , eseguire `dir Cert:\LocalMachine\My`e quindi selezionare l'identificazione personale del certificato SSL. Il valore di **< federation\_service\_name >** è il nome del servizio federativo, ad esempio FS.contoso.com.

        > [!NOTE]
        > Se non è la prima volta che si esegue questo comando, aggiungere il parametro `OverwriteConfiguration`.

        > [!NOTE]
        > Il comando precedente crea una farm WID. Se si desidera creare una farm di SQL Server, è necessario disporre dell'istanza di SQL Server già installata e operativa.
        >
        > È possibile usare il comando seguente per creare il primo server federativo in una nuova farm che usa un'istanza di SQL Server: `Install-AdfsFarm -CertificateThumbprint <certificate_thumbprint> -FederationServiceName <federation_service_name> -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` dove **sql\_Host\_Name** è il nome del server in cui è in esecuzione SQL Server e\_**nome dell'istanza di SQL\_** è il nome dell'istanza di SQL Server. Se si utilizza l'istanza predefinita di SQL Server, utilizzare un valore **sqlConnectionString** "**Data Source\=< SQL\_host\_nome >; Integrated Security\=true**".

        > [!IMPORTANT]
        > Se si desidera creare una farm AD FS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni più recenti, tra cui SQL Server 2012 e SQL Server 2014.

## <a name="BKMK_2"></a>Aggiungere un server federativo a un server farm di federazione esistente

> [!IMPORTANT]
> Assicurarsi di aver completato [il passaggio 3: installare il servizio ruolo ad FS](../../ad-fs/deployment/Install-the-AD-FS-Role-Service.md)prima di avviare una delle procedure descritte in questa sezione.

> [!IMPORTANT]
> Assicurarsi di aver ottenuto un certificato di autenticazione server SSL valido prima di completare questa procedura.

### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-the-active-directory-federation-service-configuration-wizard"></a>Per aggiungere un server federativo a una server farm federativa esistente tramite la configurazione guidata di Active Directory Federation Service

1.  Nella pagina **Dashboard** di Server Manager fare clic sul flag **Notifiche** e quindi su **Configurare il servizio federativo nel server**.

    Viene aperta la **Configurazione guidata Servizi di dominio Active Directory**.

2.  Nella pagina **iniziale** selezionare **Aggiungi un server federativo a un server farm di Federazione**, quindi fare clic su **Avanti**.

3.  Nella pagina **connessione a servizi di** dominio Active Directory specificare un account utilizzando le autorizzazioni di amministratore di dominio per il dominio di Active Directory a cui viene aggiunto il computer e quindi fare clic su **Avanti**.

4.  Nella pagina **Specifica farm specificare** il nome del server federativo primario in una farm che utilizza wid o specificare il nome host del database e il nome dell'istanza di database di un server farm di federazione esistente che utilizza SQL Server.

    > [!WARNING]
    > In Windows Server® 2012 R2 esiste una soluzione alternativa per specificare l'istanza predefinita di SQL Server. A tale scopo non viene utilizzata l'interfaccia utente, Usare invece i passaggi descritti in [per configurare il primo server federativo in una nuova federazione server farm tramite Windows PowerShell](Configure-a-Federation-Server.md#BKMK_3).

    > [!IMPORTANT]
    > Se si desidera creare una farm ADFS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni successive, incluso SQL Server 2012.

5.  Nella pagina **specificare il certificato SSL** importare il file con estensione PFX contenente il certificato e la chiave SSL ottenuti in precedenza. Questo è certificato di autenticazione del servizio obbligatorio. In [passaggio 2: registrare un certificato SSL per ad FS](../../ad-fs/deployment/Enroll-an-SSL-Certificate-for-AD-FS.md), il certificato è stato ottenuto e copiato nel computer che si desidera configurare come server federativo. Per importare il file con estensione pfx tramite la procedura guidata, fare clic su **Importa** e selezionare il percorso del file. Quando richiesto, immettere la password per il file con estensione pfx.

6.  Nella pagina **Specifica account del servizio** specificare lo stesso account di servizio configurato durante la creazione del primo server federativo nella farm. È possibile utilizzare un account del servizio gestito di gruppo esistente oppure un account utente di dominio esistente.

    > [!IMPORTANT]
    > L'account specificato deve essere lo stesso account usato nel server federativo primario di questa farm...

7.  Nella pagina **Verifica opzioni** verificare le opzioni di configurazione selezionate e quindi fare clic su **Avanti**.

8.  Nella pagina **controlli dei prerequisiti Pre\-** verificare che tutti i controlli dei prerequisiti siano stati completati correttamente e quindi fare clic su **Configura**.

9. Nella pagina **risultati** esaminare i risultati e verificare se la configurazione è stata completata correttamente e quindi fare clic su **passaggi successivi necessari per completare la distribuzione del servizio federativo**. Per ulteriori informazioni, vedere [passaggi successivi per il completamento dell'installazione di ad FS](https://go.microsoft.com/fwlink/p/?LinkId=286704). Fare clic su **Chiudi** per uscire dalla procedura guidata.

### <a name="to-add-a-federation-server-to-an-existing-federation-server-farm-via-windows-powershell"></a>Per aggiungere un server federativo a una server farm federativa esistente tramite Windows PowerShell
È possibile aggiungere un server federativo a una farm esistente usando un account gMSA esistente o un account utente di dominio esistente.

-   Se si desidera aggiungere un server federativo a una farm utilizzando un account gMSA esistente, eseguire le operazioni seguenti:

    1.  Nel computer che si desidera configurare come server federativo, verificare che il certificato SSL richiesto sia stato importato nel **computer locale\\directory archivio** . È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra di comando di Windows PowerShell: `dir Cert:\LocalMachine\My`. Il certificato è elencato in base all'identificazione personale nel **computer locale\\** directory dell'archivio.

    2.  Nel computer che si desidera configurare come server federativo aprire la finestra di comando di Windows PowerShell ed eseguire il comando seguente.

        ```
        Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>
        ```

        `<domain>\<GMSA_name>` è il dominio di Active Directory e il nome dell'account gMSA in tale dominio. `<first_federation_server_hostname>` è il nome host del server federativo primario nella farm esistente.

        È possibile ottenere il valore per `<certificate_thumbprint>` eseguendo `dir Cert:\LocalMachine\My` nel passaggio precedente.

        > [!NOTE]
        > Se non è la prima volta che si esegue questo comando, aggiungere il parametro `OverwriteConfiguration`.

        > [!NOTE]
        > Il comando precedente crea un nodo della farm WID. Se si desidera creare un server farm nodo dei computer che eseguono SQL Server, è necessario disporre dell'istanza di SQL Server già installato e operativo.
        >
        > È possibile utilizzare il comando seguente per aggiungere un server federativo a una farm esistente in cui viene utilizzata un'istanza di SQL Server: `Add-AdfsFarmNode -GroupServiceAccountIdentifier <domain>\<GMSA_name>$ -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` dove **sql\_Host\_Name** è il nome del server in cui è in esecuzione SQL Server e\_**nome dell'istanza di SQL\_** è il nome dell'istanza di SQL Server. Se si utilizza l'istanza predefinita di SQL Server, utilizzare un valore **sqlConnectionString** "**Data Source\=< SQL\_host\_nome >; Integrated Security\=true**".

        > [!IMPORTANT]
        > Se si desidera creare una farm AD FS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni più recenti, tra cui SQL Server 2012 e SQL Server 2014.

-   Se si desidera aggiungere un server federativo a una farm utilizzando un account utente di dominio esistente, effettuare le seguenti operazioni:

    1.  Nel computer che si desidera configurare come server federativo aprire la finestra PowerShellcommand Windows e quindi eseguire il comando seguente: `$fscred = get-credential`. Immettere le credenziali dell'account utente di dominio che si desidera utilizzare per l'account del servizio federativo nel formato dominio\\nome utente.

    2.  Nel computer che si desidera configurare come server federativo, verificare che il certificato SSL richiesto sia stato importato nel **computer locale\\directory archivio** . È possibile verificare se il certificato SSL è stato importato eseguendo il comando seguente nella finestra PowerShellcommand di Windows: `dir Cert:\LocalMachine\My`. Il certificato è elencato in base all'identificazione personale nel **computer locale\\** directory dell'archivio.

    3.  Nella stessa finestra di comando di Windows PowerShell, eseguire il comando seguente.

        ```
        Add-AdfsFarmNode -ServiceAccountCredential $fscred -PrimaryComputerName <first_federation_server_hostname> -CertificateThumbprint <certificate_thumbprint>
        ```

        > [!NOTE]
        > Se non è la prima volta che si esegue questo comando, aggiungere il parametro `OverwriteConfiguration`.

        > [!NOTE]
        > Il comando precedente crea un nodo della farm WID. Se si desidera creare un server farm nodo dei computer che eseguono SQL Server, è necessario disporre dell'istanza di SQL Server già installato e operativo. È possibile utilizzare il comando seguente per aggiungere un server federativo a una farm esistente utilizzando un'istanza di SQL Server: `Add-AdfsFarmNode -ServiceAccountCredential $fscred -SQLConnectionString "Data Source=<SQL_Host_Name>\<SQL_instance_ name>;Integrated Security=True"` dove **SQL\_Host\_Name** è il nome del server in cui è in esecuzione l'istanza di SQL Server e **SQL\_instance\_Name** è il nome dell'istanza di SQL Server. Se si utilizza l'istanza predefinita di SQL Server, utilizzare un valore **sqlConnectionString** "**Data Source\=< SQL\_host\_nome >; Integrated Security\=true**".

        > [!IMPORTANT]
        > Se si desidera creare una farm AD FS e utilizzare SQL Server per archiviare i dati di configurazione, è possibile utilizzare SQL Server 2008 e versioni più recenti, tra cui SQL Server 2012 e SQL Server 2014.

## <a name="see-also"></a>Vedere anche

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)

[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)


