---
title: Aggiornamento a AD FS in Windows Server 2016 con SQL Server
description: ''
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: dd843724faf1c7a8101def84091484a5e7f7900f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408233"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>Aggiornamento a AD FS in Windows Server 2016 con SQL Server


> [!NOTE]  
> Avviare un aggiornamento solo con un intervallo di tempo definitivo pianificato per il completamento. Non è consigliabile mantenere AD FS in uno stato in modalità mista per un periodo di tempo prolungato, lasciando AD FS in uno stato in modalità mista può causare problemi con la farm.


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Lo spostamento da una farm di Windows Server 2012 R2 AD ADFS in una farm ADFS a Windows Server 2016  
Nel documento seguente viene descritto come aggiornare la farm di AD FS Windows Server 2012 R2 per AD FS in Windows Server 2016 quando si utilizza un SQL Server per il database AD FS.  

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

Nel diagramma dell'architettura seguente viene illustrata la configurazione utilizzata per convalidare e registrare i passaggi riportati di seguito.

![Architecture](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Aggiungere il server di AD FS Windows 2016 alla farm AD FS

1.  Utilizzando Server Manager installa il ruolo di Active Directory Federation Services in Windows Server 2016  

2.  Utilizzando la procedura guidata configurazione di ADFS, aggiungere il nuovo server di Windows Server 2016 alla farm ADFS esistente.  Nella schermata **iniziale** fare clic su **Avanti**.
 ![join della farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  Nella schermata **Connetti a Active Directory Domain Services** , s**pecificare un account amministratore** con le autorizzazioni per eseguire la configurazione di Federation Services e fare clic su **Avanti**.
4.  Nella schermata **specifica Farm** immettere il nome dell'istanza di SQL Server e quindi fare clic su **Avanti**.
![join della farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  Nella schermata **specificare il certificato SSL** specificare il certificato e fare clic su **Avanti**.
![join della farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  Nella schermata **Specifica account del servizio** , specificare l'account del servizio e fare clic su **Avanti**.
7.  Nella schermata **verifica opzioni** rivedere le opzioni e fare clic su **Avanti**.
8.  Nella schermata **controlli dei prerequisiti** verificare che tutti i controlli dei prerequisiti siano stati superati e fare clic su **Configura**.
9.  Nella schermata **dei risultati** verificare che il server sia stato configurato correttamente e fare clic su **Chiudi**.


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Rimuovere il server AD FS Windows Server 2012 R2

>[!NOTE]
>Non è necessario impostare il server di AD FS primario utilizzando set-AdfsSyncProperties-Role quando si utilizza SQL come database.  Questo perché tutti i nodi sono considerati primari in questa configurazione.

1.  Nel server AD FS Windows Server 2012 R2 Server Manager utilizzare **Rimuovi ruoli e funzionalità** in **Gestisci**.
![rimuovere](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png) server
2.  Nella schermata **Prima di iniziare** fare clic su **Avanti**.
3.  Nella schermata **Selezione server** fare clic su **Avanti**.
4.  Nella schermata **ruoli server** , rimuovere il segno di spunta accanto a **Active Directory Federation Services** e fare clic su **Avanti**.
![rimuovere](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png) server
5.  Nella schermata **funzionalità** fare clic su **Avanti**.
6.  Nella schermata di **conferma** fare clic su **Rimuovi**.
7.  Al termine dell'operazione, riavviare il server.

#### <a name="raise-the-farm-behavior-level-fbl"></a>Aumentare il livello di comportamento della farm (FBI)
Prima di questo passaggio è necessario assicurarsi che ForestPrep e DomainPrep siano stati eseguiti nell'ambiente Active Directory e che Active Directory disponga dello schema di Windows Server 2016.  Questo documento è stato avviato con un controller di dominio Windows 2016 e non è necessario eseguirlo perché è stato eseguito quando è stato installato Active Directory.

>[!NOTE]
>Prima di avviare il processo seguente, verificare che Windows Server 2016 sia aggiornato eseguendo Windows Update da impostazioni.  Continua questo processo fino a quando non sono necessari altri aggiornamenti.

1. A questo punto, nel server Windows Server 2016 aprire PowerShell ed eseguire il comando seguente: **$cred = Get-Credential** e premere INVIO.
2. Immettere le credenziali con privilegi di amministratore per la SQL Server.
3. A questo punto, in PowerShell immettere quanto segue: **Invoke-AdfsFarmBehaviorLevelRaise-Credential $cred**
2. Quando richiesto, digitare **Y**.  Verrà avviata la generazione del livello.  Questo termine si sono generati correttamente il FBL.  
](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png) di aggiornamento ![Finish
3. A questo punto, se si passa alla gestione di ADFS, si noterà nuovi nodi che sono state aggiunte per ADFS in Windows Server 2016  
4. Analogamente, è possibile utilizzare il cmdlet PowerShell: Get-AdfsFarmInformation per mostrarvi FBL corrente.  
![Fine aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>Aggiornare la versione di configurazione dei server WAP esistenti
1. In ogni proxy applicazione Web, riconfigurare il WAP eseguendo il comando di PowerShell seguente in una finestra con privilegi elevati:  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
2. Rimuovere i server obsoleti dal cluster e conservare solo i server WAP che eseguono la versione più recente del server, che sono stati riconfigurati in precedenza, eseguendo il cmdlet di PowerShell seguente.
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. Controllare la configurazione WAP eseguendo Get-WebApplicationProxyConfiguration commmandlet. Il ConnectedServersName rifletterà l'esecuzione del server dal comando precedente.
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. Per aggiornare il ConfigurationVersion dei server WAP, eseguire il comando di PowerShell seguente.
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. Verificare che ConfigurationVersion sia stato aggiornato con il comando di PowerShell Get-WebApplicationProxyConfiguration.
