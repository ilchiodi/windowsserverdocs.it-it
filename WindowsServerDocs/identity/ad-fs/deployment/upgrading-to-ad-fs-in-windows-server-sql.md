---
title: L'aggiornamento a AD FS in Windows Server 2016 con SQL Server
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 034d4f1f8d81cf105ba94bff34b180555702acde
ms.sourcegitcommit: 33c1f4965cd2eed7d384a096cfa5c883467b16a4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/02/2017
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>L'aggiornamento a AD FS in Windows Server 2016 con SQL Server

>Si applica a: Windows Server 2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Lo spostamento da una farm di Windows Server 2012 R2 AD ADFS in una farm ADFS a Windows Server 2016  
In questo documento viene descritto come eseguire l'aggiornamento della farm di AD FS Windows Server 2012 R2 ad ADFS in Windows Server 2016, quando si utilizza un Server SQL per il database di ADFS.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>L'aggiornamento di ADFS a Windows Server 2016 FBL  
Novità in AD FS per Windows Server 2016 è la funzionalità di livello il comportamento di farm (FBL).   Questa funzionalità è per l'intera farm e determina le funzionalità che è possibile utilizzare la farm ADFS.   Per impostazione predefinita, il FBL in una farm di Windows Server 2012 R2 AD ADFS si trova il FBL di Windows Server 2012 R2.  

Un server ADFS di Windows Server 2016 può essere aggiunto a una farm di Windows Server 2012 R2 e sarà attivo il fbl stesso come un Windows Server 2012 R2.  Quando si dispone di un server ADFS di Windows Server 2016 opera in questo modo, la farm viene detto "misto".  Tuttavia, non sarà in grado di sfruttare le nuove funzionalità di Windows Server 2016 fino a quando non viene generato il FBL a Windows Server 2016.  Con una farm mista:  

-   Gli amministratori possono aggiungere nuovi server federativi di Windows Server 2016 a una farm di Windows Server 2012 R2 esistente.  Di conseguenza, la farm in "modalità mista" e opera a livello di comportamento farm di Windows Server 2012 R2.  Per garantire un comportamento coerente nella farm, nuove funzionalità di Windows Server 2016 non possono essere configurate o utilizzata in questa modalità.  

-   Una volta tutti i server federativi di Windows Server 2012 R2 sono stati rimossi dalla farm in modalità mista e in caso di una farm database interno di Windows, uno dei nuovi server federativi 2016 servire Windows sia stato promosso al ruolo del nodo primario, l'amministratore può quindi generare FBL da Windows Server 2012 R2 a Windows Server 2016.  Di conseguenza, le nuove funzionalità di AD FS Windows Server 2016 possono essere configurate e utilizzate.  

-   Di conseguenza della funzionalità di farm misto, AD FS Windows Server 2012 R2, le organizzazioni che desiderano eseguire l'aggiornamento a Windows Server 2016 non saranno necessario distribuire una farm completamente nuova, esportare e importare i dati di configurazione.  Al contrario, possono aggiungere Windows Server 2016 nodi a una farm esistente mentre è in linea e comportano solo il tempo di inattività relativamente breve coinvolti nella raise FBL.  

Tenere presente che in modalità mista farm, la farm di ADFS non è in grado di nuove funzionalità o funzionalità introdotta in ADFS in Windows Server 2016.  Questo significa che le organizzazioni che vogliono provare le nuove funzionalità non finché non viene generato il FBL.  Pertanto, se l'organizzazione esegue la ricerca per testare le nuove funzionalità prima di FBL rasing, è necessario distribuire una farm separata per eseguire questa operazione.  

La parte restante del documento fornisce i passaggi per aggiungere un server federativo di Windows Server 2016 a un ambiente Windows Server 2012 R2 e quindi di aumentarne il FBL a Windows Server 2016.  Questi passaggi sono stati eseguiti in un ambiente di test descritto nel diagramma dell'architettura riportato di seguito.  

> [!NOTE]  
> Prima di poter passare ad ADFS in Windows Server 2016 FBL, è necessario rimuovere tutti i nodi di Windows 2012 R2.  Non appena l'aggiornamento di un sistema operativo di Windows Server 2012 R2 a Windows Server 2016 ed è diventato un nodo 2016.  È necessario rimuoverlo e sostituirlo con un nuovo nodo 2016.  

Diagramma dell'architettura seguente mostra il programma di installazione che è stato usato per convalidare e registrare la procedura seguente.

![Architettura](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png) 


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Aggiungere il Server ADFS 2016 Windows per la farm di ADFS

1.  Utilizzando Server Manager installa il ruolo di servizi di Active Directory Federation in Windows Server 2016  

2.  Utilizzando la procedura guidata configurazione di ADFS, aggiungere il nuovo server Windows Server 2016 alla farm ADFS esistente.  Nel **iniziale** fare clic su schermo **Avanti**.
 ![Aggiunta di farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  Nel **connettersi a servizi di dominio Active Directory** dello schermo, s**specificare un account amministratore** con le autorizzazioni per eseguire la configurazione di servizi di federazione e fare clic su **Avanti**.
4.  Nel **specificare Farm** schermata, immettere il nome di istanza di SQL server e quindi fare clic su **Avanti**.
![Aggiunta di farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  Nel **specifica il certificato SSL** schermata, specificare il certificato e fare clic su **Avanti**.
![Aggiunta di farm](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  Nel **impostazione Account del servizio** schermata, specificare l'account del servizio e fare clic su **Avanti**. 
7.  Nel **verifica opzioni** schermata, esaminare le opzioni e fare clic su **Avanti**. 
8.  Nel **Controlla prerequisiti** schermata, assicurarsi che tutti i controlli dei prerequisiti siano stati superati e fare clic su **configura**.
9.  Nel **risultati** schermata, assicurarsi che il server è stato configurato correttamente e fare clic su **Chiudi**.
 
   
#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Rimuovere il server Windows Server 2012 R2 AD ADFS

>[!NOTE]
>Non è necessario impostare il server ADFS primario utilizzando Set-AdfsSyncProperties-ruolo quando si utilizza SQL del database.  Questo avviene perché tutti i nodi sono considerati primari in questa configurazione.

1.  Nel server di Windows Server 2012 R2 AD ADFS in Server Manager utilizzare **Rimuovi ruoli e funzionalità** in **Gestisci**. 
![Rimuovere server](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  Nel **prima di iniziare** schermata, fare clic su **Avanti**.
3.  Nel **selezione dei Server** schermata, fare clic su **Avanti**.
4.  Nel **ruoli Server** schermata, rimuovere il segno di spunta accanto a **Active Directory Federation Services** e fare clic su **Avanti**.
![Rimuovere server](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  Nel **funzionalità** schermata, fare clic su **Avanti**.
6.  Nel **conferma** schermata, fare clic su **rimuovere**.
7.  Una volta completata l'operazione, riavviare il server.
     
#### <a name="raise-the-farm-behavior-level-fbl"></a>Aumentare il livello di comportamento Farm (FBL)
Prima di questo passaggio è necessario assicurarsi che forestprep e domainprep sono stati eseguiti in ambiente Active Directory e Active Directory con lo schema di Windows Server 2016.  Questo documento avviata con un controller di dominio Windows 2016 e non richiedono che eseguono questi perché che sono stati eseguiti durante l'installazione di Active Directory.

1. Ora nel Server di Windows Server 2016, aprire PowerShell ed eseguire il comando seguente: **$cred = Get-Credential** e premere INVIO.
2. Immettere le credenziali con privilegi di amministratore in SQL Server.
3. Ora in PowerShell, immettere quanto segue: **AdfsFarmBehaviorLevelRaise Invoke-Credential $cred**
2. Quando richiesto, digitare **Y**. Verrà avviata l'aumento del livello.  Questo termine si è generati correttamente il FBL.  
![Completare l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. A questo punto, se si passa alla gestione di ADFS, si noterà nuovi nodi che sono state aggiunte per ADFS in Windows Server 2016  
4. Analogamente, è possibile utilizzare il cmdlet PowerShell: Get-AdfsFarmInformation per mostrarvi FBL corrente.  
![Completare l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)
