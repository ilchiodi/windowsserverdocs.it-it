---
title: Migrazione del database WSUS da (database interno di Windows) WID a SQL
description: "Argomento Windows Server Update Service (WSUS): come eseguire la migrazione del database WSUS (SUSDB) da un'istanza di database interno di Windows a un'istanza locale o remota di SQL Server."
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09g7dr
author: coreyp-at-msft
ms.author: coreyp
manager: dougkim
ms.date: 07/25/2018
ms.openlocfilehash: facd846dd0c20ee2e5001b0592651ce310e19097
ms.sourcegitcommit: 29f7a4811b4d36d60b8b7c55ce57d4ee7d52e263
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/20/2020
ms.locfileid: "83716886"
---
# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>Migrazione del database WSUS da WID a SQL

> Si applica a: Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Utilizzare la procedura seguente per eseguire la migrazione del database WSUS (SUSDB) da un'istanza di database interno di Windows a un'istanza locale o remota di SQL Server.

## <a name="prerequisites"></a>Prerequisiti

- Istanza di SQL. Può trattarsi del valore predefinito **MSSQLSERVER** o di un'istanza personalizzata.
- SQL Server Management Studio
- WSUS con ruolo WID installato
- IIS (questo è in genere incluso quando si installa WSUS tramite Server Manager). Poiché non è già installato, sarà necessario.

## <a name="migrating-the-wsus-database"></a>Migrazione del database WSUS

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>Arrestare i servizi IIS e WSUS sul server WSUS

Da PowerShell (con privilegi elevati) eseguire:

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>Scollegare SUSDB dal database interno di Windows

#### <a name="using-sql-management-studio"></a>Uso di SQL Management Studio

1. Fare clic con il pulsante destro del mouse su attività **SUSDB** - &gt; **Tasks** - &gt; clic su **Disconnetti**: ![ image1](images/image1.png)
2. Selezionare **Elimina connessioni esistenti** e fare clic su **OK** (facoltativo, se esistono connessioni attive).
    ![image2](images/image2.png)

#### <a name="using-command-prompt"></a>Dal prompt dei comandi

> [!IMPORTANT]
> In questa procedura viene illustrato come scollegare il database WSUS (SUSDB) dall'istanza di database interno di Windows tramite l'utilità **SQLCMD** . Per ulteriori informazioni sull'utilità **SQLCMD** , vedere [utilità sqlcmd](https://go.microsoft.com/fwlink/?LinkId=81183).
> 1. Aprire un prompt dei comandi con privilegi elevati
> 2. Eseguire il comando SQL seguente per scollegare il database WSUS (SUSDB) dall'istanza di database interno di Windows tramite l'utilità **SQLCMD** :

```batchfile
        sqlcmd -S \\.\pipe\Microsoft##WID\tsql\query
        use master
        GO
        alter database SUSDB set single_user with rollback immediate
        GO
        sp_detach_db SUSDB
        GO
```

### <a name="copy-the-susdb-files-to-the-sql-server"></a>Copiare i file SUSDB nel SQL Server

1. Copiare **SUSDB. MDF** e **SUSDB \_ log. ldf** dalla cartella wid data (**% SystemDrive%** \\ **Windows \\ wid \\ Data**) alla cartella dati dell'istanza di SQL.

> [!TIP]
> Se ad esempio la cartella dell'istanza di SQL è **C:\Programmi\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL**e la cartella wid data sono **C:\Windows\WID\Data,** copiare i file SUSDB da **C:\Windows\WID\Data** in **c:\Programmi\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>Alleghi SUSDB all'istanza di SQL

1. In **SQL Server Management Studio**, nel nodo **istanza** , fare clic con il pulsante destro del mouse su **database**, quindi scegliere **Connetti**.
    ![immagine3](images/image3.png)
2. Nella casella **Connetti database** , in **database da alleghi**, fare clic sul pulsante **Aggiungi** e individuare il file **SUSDB. MDF** copiato dalla cartella wid, quindi fare clic su **OK**.
    ![](images/image4.png) ![ image5 image4](images/image5.png)

> [!TIP]
> Questa operazione può essere eseguita anche tramite Transact-SQL.  Per le istruzioni, vedere la [documentazione di SQL per il fissaggio di un database](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) .
>
> Esempio (uso dei percorsi dell'esempio precedente):
> ```sql
>    USE master;
>    GO
>    CREATE DATABASE SUSDB
>    ON
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Data\SUSDB.mdf'),
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Log\SUSDB_Log.ldf')
>        FOR ATTACH;
>    GO
>```

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>Verificare gli account di accesso e le autorizzazioni di SQL Server e database

#### <a name="sql-server-login-permissions"></a>Autorizzazioni di accesso SQL Server

Dopo aver collegato il SUSDB, verificare che il **servizio NT AUTHORITY\NETWORK** disponga delle autorizzazioni di accesso all'istanza di SQL Server eseguendo le operazioni seguenti:

1. Passa a SQL Server Management Studio
2. Apertura dell'istanza
3. Fare clic su **sicurezza**
4. Fare clic su **accessi**

L'account **NT Authority\Network Service** deve essere elencato. In caso contrario, è necessario aggiungerlo aggiungendo un nuovo nome di account di accesso.

> [!IMPORTANT]
> Se l'istanza SQL si trova in un computer diverso da WSUS, l'account computer del server WSUS deve essere elencato nel formato **[FQDN] \\ [WSUSComputerName] $**.  In caso contrario, è possibile utilizzare la procedura seguente per aggiungerla, sostituendo **NT Authority\Network Service** con l'account computer del server WSUS (**[FQDN] \\ [WSUSComputerName] $**), questo sarebbe ***oltre a*** concedere diritti al **servizio NT AUTHORITY\NETWORK**

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>Aggiunta di NT AUTHORITY\NETWORK SERVICE e concessione dei diritti it

1. Fare clic con il pulsante destro del mouse su **accessi** e scegliere **nuovo account di accesso.**
    ![Image6](images/image6.png)
2. Nella pagina **generale** compilare il **nome dell'account di accesso** (**NT Authority\Network Service**) e impostare il **database predefinito** su SUSDB.
    ![Image7](images/image7.png)
3. Nella pagina **ruoli server** verificare che sia selezionata l'opzione **pubblica** e **sysadmin** .
    ![Immagine8](images/image8.png)
4. Nella pagina **mapping utenti** :
    - In **utenti con mapping a questo account di accesso**: selezionare **SUSDB**
    - In **appartenenza a ruoli del database per: SUSDB**verificare che siano controllati gli elementi seguenti:
        - **pubblico**
        - Servizio Web **webService** ![ image9](images/image9.png)
5. Fare clic su **OK**.

A questo punto verrà visualizzato **NT Authority\Network Service** in account di accesso.
![Immagine10](images/image10.png)

#### <a name="database-permissions"></a>Autorizzazioni per il database

1. Fare clic con il pulsante destro del mouse su SUSDB
2. Selezione **Proprietà**
3. Fare clic su **autorizzazioni**

L'account **NT Authority\Network Service** deve essere elencato.

1. In caso contrario, aggiungere l'account.
2. Nella casella di testo nome account di accesso immettere il computer WSUS nel formato seguente:
    > [**FQDN] \\ [WSUSComputerName] $**
3. Verificare che il **database predefinito** sia impostato su **SUSDB**.

    > [!TIP]
    > Nell'esempio seguente, il nome di dominio completo è **contosto.com** e il nome del computer WSUS è **WsusMachine**:
    >
    > ![Immagine11](images/image11.png)

4. Nella pagina **mapping utenti** selezionare il database **SUSDB** in utenti con **mapping a questo account di accesso**
5. Controllare **WebService** con l' **appartenenza a ruoli del database per: SUSDB**: ![ IMAGE12](images/image12.png)
6. Fare clic su **OK** per salvare le impostazioni.
    > [!NOTE]
    > Per rendere effettive le modifiche, potrebbe essere necessario riavviare il servizio SQL.

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>Modificare il registro di sistema per puntare WSUS all'istanza di SQL Server

> [!IMPORTANT]
> Segui con attenzione la procedura descritta in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [esegui il backup del Registro di sistema per il ripristino](https://support.microsoft.com/help/322756) nel caso in cui si verifichino problemi.

1. Fare clic su **Start**, quindi su **Esegui**, digitare **regedit&** e quindi fare clic su **OK**.
2. Individuare la chiave seguente: **HKEY_LOCAL_MACHINE \software\microsoft\updateservices\server\setup\sqlservername**
3. Nella casella di testo **valore** Digitare **[nomeserver] \\ [nomeistanza]**, quindi fare clic su **OK**. Se il nome dell'istanza è l'istanza predefinita, digitare **[servername]**.
4. Individuare la chiave seguente: **HKEY_LOCAL_MACHINE \Software\microsoft\update Services\Server\Setup\Installed Role Services\UpdateServices-WidDatabase** ![ Image13](images/image13.png)
5. Rinominare la chiave in **UpdateServices-database** ![ image41](images/image14.png)

    > [!NOTE]
    > Se non si aggiorna questa chiave, **WsusUtil** tenterà di eseguire il servizio a wid anziché all'istanza di SQL in cui è stata eseguita la migrazione.

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>Avviare i servizi IIS e WSUS sul server WSUS

Da PowerShell (con privilegi elevati) eseguire:

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> Se si utilizza la console di WSUS, chiuderla e riavviarla.

## <a name="uninstalling-the-wid-role-not-recommended"></a>Disinstallazione del ruolo WID (scelta non consigliata)

> [!WARNING]
> La rimozione del ruolo WID consente inoltre di rimuovere una cartella di database (**%SystemDrive%\Program Programmi\update Services\Database**) che contiene gli script richiesti da WSUSutil. exe per le attività successive all'installazione. Se si sceglie di disinstallare il ruolo WID, assicurarsi di eseguire prima di tutto il backup della cartella **%SystemDrive%\Program Programmi\update Services\Database** .

Usando PowerShell:

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

Dopo la rimozione del ruolo WID, verificare che sia presente la seguente chiave del registro di sistema: **HKEY_LOCAL_MACHINE \Software\microsoft\update Services\Server\Setup\Installed Role Services\UpdateServices-database**
