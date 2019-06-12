---
title: Migrazione del Database WSUS dal (Database interno di Windows) a SQL database interno di Windows
description: Argomento di Windows Server Update Service (WSUS) - come eseguire la migrazione del database WSUS (SUSDB) da un'istanza di Database interno di Windows a un'istanza locale o remota di SQL Server.
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09g7dr
author: coreyp-at-msft
ms.author: coreyp
manager: dougkim
ms.date: 07/25/2018
ms.openlocfilehash: 9015bbc54a4c4bda0f691b79dbb7d3ba8ddbc4a1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439888"
---
>Si applica a: Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>La migrazione del Database WSUS da database interno di Windows per SQL

Usare la procedura seguente per eseguire la migrazione del database WSUS (SUSDB) da un'istanza di Database interno di Windows per un'istanza locale o remota di SQL Server.

## <a name="prerequisites"></a>Prerequisiti

- Istanza di SQL. Ciò può essere il valore predefinito **MSSQLServer** o un'istanza personalizzata.
- SQL Server Management Studio
- WSUS con ruolo di database interno di Windows installato
- IIS (si tratta in genere incluso quando si installa WSUS tramite Server Manager). Non è già installato, dovrà essere.

## <a name="migrating-the-wsus-database"></a>Eseguire la migrazione del database WSUS

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>Arrestare i servizi IIS e Windows Server Update Services nel server WSUS

Da PowerShell (con privilegi elevati), eseguire:

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>Scollegare SUSDB dal Database interno di Windows

#### <a name="using-sql-management-studio"></a>Usando SQL Management Studio

1. Fare doppio clic su **SUSDB** - &gt; **le attività** - &gt; fare clic su **Detach**: ![image1](images/image1.png)
2. Controllare **Interrompi connessioni esistenti** e fare clic su **OK** (facoltativo, se esistono connessioni attive).
    ![image2](images/image2.png)

#### <a name="using-command-prompt"></a>Dal prompt dei comandi

> [!IMPORTANT]
> Questi passaggi illustrano come scollegare il database WSUS (SUSDB) dall'istanza del Database interno di Windows usando il **sqlcmd** utilità. Per altre informazioni sul **sqlcmd** utilità, vedere [utilità sqlcmd](https://go.microsoft.com/fwlink/?LinkId=81183).
> 1. Aprire un prompt dei comandi con privilegi elevati
> 2. Eseguire il comando SQL seguente per scollegare il database WSUS (SUSDB) dall'istanza del Database interno di Windows usando il **sqlcmd** utilità:

```batchfile
        sqlcmd -S \\.\pipe\Microsoft##WID\tsql\query
        use master
        GO
        alter database SUSDB set single_user with rollback immediate
        GO
        sp_detach_db SUSDB
        GO
```

### <a name="copy-the-susdb-files-to-the-sql-server"></a>Copiare i file SUSDB a SQL Server

1. Copia **SUSDB. mdf** e **SUSDB\_log. ldf** dalla cartella di dati di database interno di Windows ( **% SystemDrive %** \** Windows\WID\Data * *) per i dati dell'istanza SQL Cartella.

> [!TIP]
> Ad esempio, se la cartella dell'istanza SQL è **C:\Program Files\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL**, ed è la cartella dati WID **C:\Windows\WID\Data,** copiare i file SUSDB dalla **C:\Windows\WID\Data** a **c:\Programmi\Microsoft SQL Server \MSSQL12. MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>Collegare SUSDB all'istanza di SQL

1. Nella **SQL Server Management Studio**, sotto il **istanza** nodo, fare doppio clic su **database**e quindi fare clic su **Attach**.
    ![image3](images/image3.png)
2. Nel **Collega database** finestra di **database da collegare**, fare clic sul **Add** pulsante e individuare il **SUSDB. mdf** file (copiati dal Cartella WID), quindi fare clic su **OK**.
    ![image4](images/image4.png) ![image5](images/image5.png)

> [!TIP]
> Ciò può anche essere eseguita tramite Transact-Sql.  Vedere le [documentazione di SQL per collegare un database](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) per le proprie istruzioni.
>
> Esempio (con i percorsi dall'esempio precedente):
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

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>Verificare SQL Server e gli account di accesso di Database e autorizzazioni

#### <a name="sql-server-login-permissions"></a>Autorizzazioni di accesso SQL Server

Dopo aver collegato il database WSUS, verificare che **NT AUTHORITY\NETWORK SERVICE** disponga delle autorizzazioni di accesso per l'istanza di SQL Server eseguendo le operazioni seguenti:

1. Andare in SQL Server Management Studio
2. Aprire l'istanza
3. Fare clic su **sicurezza**
4. Fare clic su **gli account di accesso**

Il **NT AUTHORITY\NETWORK SERVICE** account dovrebbe essere elencato. In caso contrario, è necessario aggiungerlo mediante l'aggiunta di nuovo nome di accesso.

> [!IMPORTANT]
> Se l'istanza di SQL è in un computer diverso da Windows Server Update Services, account computer del Server WSUS dovrebbe essere elencato nel formato **[FQDN]\\[WSUSComputerName] $** .  Se non, la procedura seguente consente di aggiungere, sostituzione **NT AUTHORITY\NETWORK SERVICE** con l'account computer del Server WSUS ( **[FQDN]\\[WSUSComputerName] $** ) sarebbe ***oltre a*** concessione dei diritti al **NT AUTHORITY\NETWORK SERVICE**

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>Aggiungere NT AUTHORITY\NETWORK SERVICE e concedendo i diritti

1. Fare clic destro **gli account di accesso** e fare clic su **nuovo account di accesso...**
    ![image6](images/image6.png)
2. Nel **generali** pagina, compilare il **nome account di accesso** (**NT AUTHORITY\NETWORK SERVICE**) e impostare il **database predefinito** a SUSDB.
    ![image7](images/image7.png)
3. Nel **ruoli predefiniti del Server** pagina, assicurarsi **pubblici** e **sysadmin** siano selezionate.
    ![image8](images/image8.png)
4. Nel **Mapping utenti** pagina:
    - Sotto **utenti mappati all'account di accesso seguente**: selezionare **SUSDB**
    - In **Database l'appartenenza al ruolo per: SUSDB**, assicurarsi che siano selezionate le seguenti:
        - **public**
        - **webService** ![image9](images/image9.png)
5. Fare clic su **OK**.

A questo punto dovrebbe **NT AUTHORITY\NETWORK SERVICE** in account di accesso.
![image10](images/image10.png)

#### <a name="database-permissions"></a>Autorizzazioni del database

1. Fare doppio clic il database WSUS
2. Selezionare **proprietà**
3. Fare clic su **autorizzazioni**

Il **NT AUTHORITY\NETWORK SERVICE** account dovrebbe essere elencato.

1. In caso contrario, aggiungere l'account.
2. Nella casella di testo Nome account di accesso, immettere il computer WSUS nel formato seguente:
    > [**FQDN]\\[WSUSComputerName]$**
3. Verificare che il **database predefinito** è impostata su **SUSDB**.

    > [!TIP]
    > Nell'esempio seguente, è il nome FQDN **Contosto.com** ed è il nome del computer WSUS **WsusMachine**:
    >
    > ![Image11](images/image11.png)

4. Nel **Mapping utenti** pagina, selezionare la **SUSDB** Database sotto **"Utenti mappati all'account di accesso"**
5. Controllare **webservice** sotto il **"Database di appartenenza al ruolo per: SUSDB"** :  ![image12](images/image12.png)
6. Fare clic su **OK** per salvare le impostazioni.
    > [!NOTE]
    > Si potrebbe essere necessario riavviare il servizio SQL per rendere effettive le modifiche.

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>Modificare il Registro di sistema al punto di WSUS per l'istanza di SQL Server

> [!IMPORTANT]
> Seguire con attenzione i passaggi descritti in questa sezione. Se le modifiche al Registro di sistema vengono apportate in modo non corretto, possono verificarsi problemi gravi. Prima di modificarlo, [eseguire il backup Registro di sistema per il ripristino](https://support.microsoft.com/en-us/help/322756) nel caso in cui si verificano problemi.

1. Fare clic su **Start**, **Esegui**, digitare **regedit** e fare clic su **OK**.
2. Individuare la chiave seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\UpdateServices\Server\Setup\SqlServerName**
3. Nel **valore** casella di testo, digitare **[nomeserver]\\[nomeistanza]** , quindi fare clic su **OK**. Se il nome dell'istanza è l'istanza predefinita, digitare **[nomeserver]** .
4. Individuare la chiave seguente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed ruolo Services\UpdateServices-WidDatabase** ![image13](images/image13.png)
5. La chiave da rinominare **UpdateServices-Database** ![image41](images/image14.png)

    > [!NOTE]
    > Se non si aggiorna questa chiave, quindi **WsusUtil** tenterà il database interno di Windows anziché l'istanza di SQL a cui è stata eseguita la migrazione del servizio.

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>Avviare i servizi IIS e Windows Server Update Services nel server WSUS

Da PowerShell (con privilegi elevati), eseguire:

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> Se si usa la Console di WSUS, chiudere e riavviarlo.

## <a name="uninstalling-the-wid-role-not-recommended"></a>Disinstallare il ruolo di database interno di Windows (scelta non consigliato)

> [!WARNING]
> La rimozione del ruolo di database interno di Windows comporta anche una cartella di database ( **%systemdrive%\Programmi\Microsoft Files\Update Services\Database**) che contiene gli script necessari da WSUSUtil.exe per le attività di post-installazione. Se si sceglie di disinstallare il ruolo del database interno di Windows, assicurarsi di eseguire il backup di **%systemdrive%\Programmi\Microsoft Files\Update Services\Database** cartella in anticipo.

Utilizzo di PowerShell:

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

Al termine viene rimosso il ruolo del database interno di Windows, verificare che la chiave del Registro di sistema seguente sia presente: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed ruolo Services\UpdateServices-Database**