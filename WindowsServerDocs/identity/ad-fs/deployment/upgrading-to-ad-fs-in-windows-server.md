---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: L'aggiornamento a AD FS in Windows Server 2016
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ce07398a2d624a1e9b004cd35eb9228d59dc2b5b
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2018
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>L'aggiornamento a AD FS in Windows Server 2016 tramite un database interno di Windows

>Si applica a: Windows Server 2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>Lo spostamento da una farm di Windows Server 2012 R2 AD ADFS in una farm ADFS a Windows Server 2016  
In questo documento viene descritto come eseguire l'aggiornamento della farm di AD FS Windows Server 2012 R2 ad ADFS in Windows Server 2016, quando si utilizza un database interno di Windows.  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>L'aggiornamento di ADFS a Windows Server 2016 FBL  
Novità in AD FS per Windows Server 2016 è la funzionalità di livello il comportamento di farm (FBL).   Questa funzionalità è per l'intera farm e determina le funzionalità che è possibile utilizzare la farm ADFS.   Per impostazione predefinita, il FBL in una farm di Windows Server 2012 R2 AD ADFS si trova il FBL di Windows Server 2012 R2.  

Un server ADFS di Windows Server 2016 può essere aggiunto a una farm di Windows Server 2012 R2 e sarà attivo il fbl stesso come un Windows Server 2012 R2.  Quando si dispone di un server ADFS di Windows Server 2016 opera in questo modo, la farm viene detto "misto".  Tuttavia, non sarà in grado di sfruttare le nuove funzionalità di Windows Server 2016 fino a quando non viene generato il FBL a Windows Server 2016.  Con una farm mista:  

-   Gli amministratori possono aggiungere nuovi server federativi di Windows Server 2016 a una farm di Windows Server 2012 R2 esistente.  Di conseguenza, la farm in "modalità mista" e opera a livello di comportamento farm di Windows Server 2012 R2.  Per garantire un comportamento coerente nella farm, nuove funzionalità di Windows Server 2016 non possono essere configurate o utilizzata in questa modalità.  

-   Una volta tutti i server federativi di Windows Server 2012 R2 sono stati rimossi dalla farm in modalità mista e in caso di una farm database interno di Windows, uno dei nuovi server federativi di Windows Server 2016 è stato promosso al ruolo del nodo primario, l'amministratore può quindi generare FBL da Windows Server 2012 R2 a Windows Server 2016.  Di conseguenza, le nuove funzionalità di AD FS Windows Server 2016 possono essere configurate e utilizzate.  

-   Di conseguenza della funzionalità di farm misto, AD FS Windows Server 2012 R2, le organizzazioni che desiderano eseguire l'aggiornamento a Windows Server 2016 non saranno necessario distribuire una farm completamente nuova, esportare e importare i dati di configurazione.  Al contrario, possono aggiungere Windows Server 2016 nodi a una farm esistente mentre è in linea e comportano solo il tempo di inattività relativamente breve coinvolti nella raise FBL.  

Tenere presente che in modalità mista farm, la farm di ADFS non è in grado di nuove funzionalità o funzionalità introdotta in ADFS in Windows Server 2016.  Questo significa che le organizzazioni che vogliono provare le nuove funzionalità non finché non viene generato il FBL.  Pertanto, se l'organizzazione esegue la ricerca per testare le nuove funzionalità prima di FBL rasing, è necessario distribuire una farm separata per eseguire questa operazione.  

La parte restante del documento fornisce i passaggi per aggiungere un server federativo di Windows Server 2016 a un ambiente Windows Server 2012 R2 e quindi di aumentarne il FBL a Windows Server 2016.  Questi passaggi sono stati eseguiti in un ambiente di test descritto nel diagramma dell'architettura riportato di seguito.  

> [!NOTE]  
> Prima di poter passare ad ADFS in Windows Server 2016 FBL, è necessario rimuovere tutti i nodi di Windows 2012 R2.  Non appena l'aggiornamento di un sistema operativo di Windows Server 2012 R2 a Windows Server 2016 ed è diventato un nodo 2016.  È necessario rimuoverlo e sostituirlo con un nuovo nodo 2016.
>
> L'aggiornamento il FBL quando si utilizza SQL per archiviare la configurazione di ADFS creare un nuovo database di gestione "AdfsConfigurationV3".

##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2016-farm-behavior-level"></a>Per aggiornare la farm ADFS a livello di comportamento Farm di Windows Server 2016  

1.  Utilizzando Server Manager installa il ruolo di servizi di Active Directory Federation in Windows Server 2016  

2.  Utilizzando la procedura guidata configurazione di ADFS, aggiungere il nuovo server Windows Server 2016 alla farm ADFS esistente.  

    ![eseguire l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  Nel server federativo di Windows Server 2016, aprire Gestione ADFS.    Si noti che non viene visualizzato come il server federativo non è il server primario.  

    ![eseguire l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  Al termine il join, nel server di Windows Server 2016, aprire PowerShell ed eseguire il seguente cmdlet: Set-AdfsSyncProperties-PrimaryComputer ruolo  

    ![eseguire l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  Nel server AD FS Windows Server 2012 R2 originale, aprire PowerShell ed eseguire il seguente cmdlet: Set-AdfsSyncProperties-ruolo SecondaryComputer - PrimaryComputerName {FQDN}  

    ![eseguire l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  Nel Proxy di applicazione Web, aprire PowerShell ed eseguire il cmdlet seguenti: {SSLCert} Install-WebApplicationProxy - CertificateThumbprint - fsname fsname - TrustCred $trustcred  

7.  Nel server federativo di Windows Server 2016 aprire Gestione ADFS.  Si noti che ora tutti i nodi vengono visualizzati perché il ruolo primario è stato trasferito a questo server.  

    ![eseguire l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

8.  Con il supporto di installazione di Windows Server 2016, aprire un prompt dei comandi e passare alla directory support\adprep..  Eseguire il comando seguente: adprep /forestprep.  

    ![eseguire l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

9. Quando è stata completata, eseguire adprep/domainprep  

    ![eseguire l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

10. Ora nel Server di Windows Server 2016, aprire PowerShell ed eseguire il cmdlet seguente: AdfsFarmBehaviorLevelRaise Invoke  

    ![eseguire l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

11. Quando richiesto, digitare Y. Verrà avviata l'aumento del livello.  Questo termine si è generati correttamente il FBL.  

    ![eseguire l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

> [!NOTE]  
> Se i server ADFS utilizzano SQL per la configurazione, con il nome "AdfsConfiguraionV3" è stato creato un nuovo database manamgement. 

12. A questo punto, se si passa alla gestione di ADFS, si noterà nuovi nodi che sono state aggiunte per ADFS in Windows Server 2016  

    ![eseguire l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

13. Analogamente, è possibile utilizzare il cmdlet PowerShell: Get-AdfsFarmInformation per mostrarvi FBL corrente.  

    ![eseguire l'aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  
