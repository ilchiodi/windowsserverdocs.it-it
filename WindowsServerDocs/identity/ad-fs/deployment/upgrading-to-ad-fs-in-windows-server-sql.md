---
title: L'aggiornamento a AD FS in Windows Server 2016 con SQL Server
description: ''
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8ada2ae5c9fcdb77f35200581848041f222ed7f3
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191961"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>L'aggiornamento a AD FS in Windows Server 2016 con SQL Server



## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Lo spostamento da una farm di Windows Server 2012 R2 AD ADFS in una farm ADFS a Windows Server 2016  
Il documento seguente viene descritto come aggiornare la farm di AD FS Windows Server 2012 R2 ad ADFS in Windows Server 2016 quando si usa un Server SQL per il database di ADFS.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>Aggiornamento di ADFS per Windows Server 2016 FBL  
Novità di ADFS per Windows Server 2016 è la funzionalità di livello il comportamento di farm (FBL).   Questa funzionalità è per l'intera farm e determina le funzionalità che consente la farm di ADFS.   Per impostazione predefinita, il FBL in una farm di Windows Server 2012 R2 AD ADFS si trova il FBL di Windows Server 2012 R2.  

Un server ADFS di Windows Server 2016 può essere aggiunto a una farm di Windows Server 2012 R2 e sarà attivo il FBL stesso come Windows Server 2012 R2.  Quando si dispone di un server ADFS di Windows Server 2016 opera in questo modo, la farm viene detto "misto".  Tuttavia, non sarà in grado di sfruttare le nuove funzionalità di Windows Server 2016 fino a quando non viene generato il FBL a Windows Server 2016.  Con una farm mista:  

-   Gli amministratori possono aggiungere nuovi server federativi di Windows Server 2016 a una farm di Windows Server 2012 R2 esistente.  Di conseguenza, la farm in "modalità mista" e opera a livello di comportamento di farm di Windows Server 2012 R2.  Per garantire un comportamento coerente nella farm, nuove funzionalità di Windows Server 2016 non può essere configurata o utilizzata in questa modalità.  

-   Dopo che tutti i server federativi di Windows Server 2012 R2 sono stati rimossi dalla farm in modalità mista e in caso di una farm database interno di Windows, uno dei nuovi server federativi 2016 servire Windows sia stato promosso al ruolo del nodo primario, l'amministratore può quindi generare FBL da Windows Server 2012 R2 a Windows Server 2016.  Di conseguenza, le nuove funzionalità di Windows Server ADFS 2016 può quindi essere configurata e utilizzata.  

-   Di conseguenza della funzionalità di farm misto, AD FS Windows Server 2012 R2, le organizzazioni che desiderano eseguire l'aggiornamento a Windows Server 2016 non saranno necessario distribuire una farm completamente nuova, esportare e importare dati di configurazione.  Invece, possono aggiungere Windows Server 2016 nodi a una farm esistente mentre è in linea e che solo il tempo di inattività relativamente breve coinvolti nella raise FBL.  

Tenere presente che in modalità mista farm, la farm di ADFS non è in grado di nuove funzionalità o funzionalità introdotta in ADFS in Windows Server 2016.  Questo significa che le organizzazioni che vogliono provare le nuove funzionalità non finché non viene generato il FBL.  Pertanto, se l'organizzazione esegue la ricerca per testare le nuove funzionalità prima di FBL rasing, è necessario distribuire una farm separata per eseguire questa operazione.  

Il resto del documento fornisce i passaggi per l'aggiunta di un server federativo di Windows Server 2016 in un ambiente Windows Server 2012 R2 e quindi di aumentarne il FBL per Windows Server 2016.  Questi passaggi sono stati eseguiti in un ambiente di test descritto nel diagramma dell'architettura riportato di seguito.  

> [!NOTE]  
> Prima di poter passare ad ADFS in Windows Server 2016 FBL, è necessario rimuovere tutti i nodi di Windows 2012 R2.  Non appena l'aggiornamento di un sistema operativo di Windows Server 2012 R2 a Windows Server 2016 ed è diventato un nodo 2016.  È necessario rimuoverlo e sostituirlo con un nuovo nodo 2016.  

Diagramma dell'architettura seguente viene illustrato il programma di installazione che è stato usato per convalidare e registrare la procedura seguente.

![Architecture](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Aggiungere il Server ADFS 2016 Windows per la farm AD FS

1.  Utilizzando Server Manager installa il ruolo di Active Directory Federation Services in Windows Server 2016  

2.  Utilizzando la procedura guidata configurazione di ADFS, aggiungere il nuovo server di Windows Server 2016 alla farm ADFS esistente.  Nel **benvenuto** dello schermo fare clic su **successivo**.
 ![Aggiungi a farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  Nel **connettersi a servizi di dominio Active Directory** dello schermo, s**pecificare un account amministratore** con le autorizzazioni per eseguire la configurazione di servizi di federazione e fare clic su **Next**.
4.  Nel **impostazione Farm** schermata, immettere il nome del server SQL e dell'istanza e quindi fare clic su **successivo**.
![Aggiungi a farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  Nel **specifica il certificato SSL** schermata, specificare il certificato e fare clic su **successivo**.
![Aggiungi a farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  Nel **impostazione Account del servizio** schermata, specificare l'account del servizio e fare clic su **successivo**.
7.  Nel **esaminare le opzioni** schermata, esaminare le opzioni e fare clic su **successivo**.
8.  Nel **prerequisiti controlla** schermata, assicurarsi che tutti i controlli dei prerequisiti sono stati superati e fare clic su **configura**.
9.  Nel **risultati** schermata, assicurarsi che tale server è stato configurato correttamente e fare clic su **Chiudi**.


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Rimuovere il server AD FS di Windows Server 2012 R2

>[!NOTE]
>Non è necessario impostare il server ADFS primario tramite Set-AdfsSyncProperties-ruolo quando si usa SQL come database.  Questo avviene perché tutti i nodi sono considerati principali in questa configurazione.

1.  Nel server di Windows Server 2012 R2 AD ADFS in uso di Server Manager **Rimuovi ruoli e funzionalità** sotto **Gestisci**.
![Rimuovi server](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  Nella schermata **Prima di iniziare** fare clic su **Avanti**.
3.  Nel **selezione del Server** schermata, fare clic su **successivo**.
4.  Nel **ruoli predefiniti del Server** schermata, rimuovere il segno di spunta accanto a **Active Directory Federation Services** e fare clic su **Next**.
![Rimuovi server](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  Nel **caratteristiche** schermata, fare clic su **successivo**.
6.  Nel **conferma** schermata, fare clic su **rimuovere**.
7.  Al termine, riavviare il server.

#### <a name="raise-the-farm-behavior-level-fbl"></a>Aumentare il livello di comportamento Farm (FBL)
Prima di questo passaggio è necessario assicurarsi che forestprep e domainprep sono stati eseguiti nell'ambiente di Active Directory e che Active Directory con lo schema di Windows Server 2016.  Questo documento avviata con un controller di dominio Windows 2016 e non era necessaria l'esecuzione di questi perché sono stati eseguiti durante l'installazione di AD.

>[!NOTE]
>Prima di iniziare la procedura seguente, verificare che Windows Server 2016 è corrente tramite l'esecuzione di Windows Update dalle impostazioni.  Continua questo processo fino a quando non sono necessari altri aggiornamenti.

1. Ora nel Server di Windows Server 2016 aprire PowerShell ed eseguire il comando seguente: **$cred = Get-Credential** e premere INVIO.
2. Immettere le credenziali con privilegi di amministratore in SQL Server.
3. Ora in PowerShell, immettere quanto segue: **Invoke-AdfsFarmBehaviorLevelRaise -Credential $cred**
2. Quando richiesto, digitare **Y**.  Verrà avviata l'aumento del livello.  Questo termine si sono generati correttamente il FBL.  
![Completare l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. A questo punto, se si passa alla gestione di ADFS, si noterà nuovi nodi che sono state aggiunte per ADFS in Windows Server 2016  
4. Analogamente, è possibile utilizzare il cmdlet di PowerShell:  Get-AdfsFarmInformation per mostrarvi FBL corrente.  
![Completare l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>Aggiornare la versione di configurazione dei server WAP esistenti
1. In ogni Proxy applicazione Web, riconfigurare il WAP eseguendo il comando PowerShell seguente in una finestra con privilegi elevata:  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
2. Rimuovere i vecchi server dal cluster e mantenere solo i server WAP in esecuzione la versione più recente di server, che sono stati riconfigurati sopra, eseguendo il commandlet di Powershell seguente.
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. Controllare la configurazione di WAP eseguendo il commmandlet Get-WebApplicationProxyConfiguration. Il ConnectedServersName rifletterà server eseguita dal comando precedente.
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. Per aggiornare la versione di configurazione dei server WAP, eseguire il comando Powershell seguente.
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. Verificare che la versione di configurazione è stata aggiornata con il comando Powershell Get-WebApplicationProxyConfiguration.
    
