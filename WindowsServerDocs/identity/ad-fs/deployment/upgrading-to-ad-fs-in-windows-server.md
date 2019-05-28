---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Aggiornamento ad AD FS in Windows Server 2016
description: ''
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c8e72f1075b984506f9f992cd45cf853b50bddeb
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191923"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>Aggiornamento ad AD FS in Windows Server 2016 con un database interno di Windows



## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>L'aggiornamento a Windows Server 2012 R2 o 2016 farm ADFS a Windows Server 2019
Il documento seguente viene descritto come eseguire l'aggiornamento della farm AD FS a AD FS in Windows Server 2019 quando si usa database interno di Windows.  

### <a name="ad-fs-farm-behavior-levels-fbl"></a>Livelli di comportamento di Farm di ADFS (FBL) AD  
In AD FS per Windows Server 2016 è stato introdotto il livello di comportamento farm (FBL). Si tratta di impostazione a livello di farm che determina che la farm di funzionalità di AD FS è possibile usare.

Nella tabella seguente sono elencati i valori FBL dalla versione di Windows Server:
| Versione di Windows Server  | FBL | Nome del Database di configurazione di AD FS |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]  
> L'aggiornamento il FBL crea un nuovo database di configurazione di AD FS.  Vedere la tabella precedente per i nomi dei database di configurazione per ogni versione di Windows Server AD FS e il valore FBL

### <a name="new-vs-upgraded-farms"></a>Visual Studio nuova farm aggiornato
Per impostazione predefinita, il FBL in una nuova farm AD FS corrisponde al valore per la versione di Windows Server del nodo della farm prima installato.  

Un server AD FS di una versione più recente può essere unito a una farm AD FS 2012 R2 o 2016 e la farm venga utilizzato il fbl stesso come dei nodi esistenti. Quando si dispone di più versioni di Windows Server operativi nella stessa farm in corrispondenza del valore FBL della versione più bassa, la farm viene detto "misto". Tuttavia, non sarà in grado di sfruttare i vantaggi delle funzionalità di versioni successive, fino a quando non viene generato il FBL. Con una farm mista:  

-   Gli amministratori possono aggiungere nuovi server federativi di Windows Server 2019 per un esistenti di Windows Server 2012 R2 o 2016 farm. Di conseguenza, la farm in "modalità mista" e opera a livello di comportamento farm stessa della farm originale. Per garantire un comportamento coerente nella farm, le funzionalità di nuove versioni Windows Server AD FS non è possibile configurare o usate.  

- Prima può essere generato il FBL, gli amministratori devono rimuovere nodi delle versioni precedenti di Windows Server AD FS dalla farm.  Nel caso di una farm database interno di Windows, si noti che questa operazione richiede una dell'elaborazione delle transazioni di nuovo Windows Server 2019 federation server alzata di livello al ruolo del nodo primario nella farm.

-   Dopo che tutti i server federativi nella farm sono della stessa versione di Windows Server, può essere generato il FBL.  Di conseguenza, eventuali nuove funzionalità di Windows Server ADFS 2019 può quindi essere configurata e usata.

Tenere presente che mentre in modalità mista farm, la farm AD FS non è in grado di nuove funzionalità o funzionalità introdotta in ADFS in Windows Server 2019. Questo significa che le organizzazioni che vogliono provare le nuove funzionalità non finché non viene generato il FBL. Pertanto, se l'organizzazione esegue la ricerca per testare le nuove funzionalità prima di FBL rasing, è necessario distribuire una farm separata per eseguire questa operazione.  

Il resto del documento fornisce i passaggi per l'aggiunta di un server federativo di Windows Server 2019 per ambiente 2012 R2 o Windows Server 2016 e quindi di aumentarne il FBL a Windows Server 2019. Questi passaggi sono stati eseguiti in un ambiente di test descritto nel diagramma dell'architettura riportato di seguito.  

> [!NOTE]  
> Prima di poter passare ad ADFS in Windows Server 2019 FBL, è necessario rimuovere tutti i Windows Server 2016 o 2012 R2 nodi. Appena non è possibile eseguire l'aggiornamento del sistema operativo R2 di 2012 o Windows Server 2016 per Windows Server 2019 ed è diventato un nodo 2019. È necessario rimuoverlo e sostituirlo con un nuovo nodo 2019.



##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>Per aggiornare la farm ADFS a livello di comportamento Farm di Windows Server 2019  

1.  Usa Server Manager, installare il ruolo di Active Directory Federation Services il 2019 di Windows Server

2.  Tramite la procedura guidata configurazione di AD FS, unire il nuovo server Windows Server 2019 alla farm ADFS esistente.  

    ![aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  Nel server federativo di Windows Server 2019, aprire Gestione ADFS. Si noti che le funzionalità di gestione non sono disponibili perché il server federativo non è il server primario.  

    ![aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  Nel server di Windows Server 2019, aprire una finestra di comando di PowerShell con privilegi elevata ed eseguire il cmdlet seguente: `Set-AdfsSyncProperties -Role PrimaryComputer`

    ![aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  Nel server AD FS che è stato precedentemente configurato come primario, aprire una finestra di comando di PowerShell con privilegi elevata ed eseguire il cmdlet seguente: `Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN} `

    ![aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  Ora sul server federativo a Windows Server 2016 aprire Gestione ADFS. Si noti che ora tutte le funzionalità di amministrazione vengono visualizzati perché il ruolo primario è stato trasferito a questo server.  

    ![aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

7.  Se si sta aggiornando una farm di AD FS 2012 R2 a 2016 o 2019, l'aggiornamento richiede lo schema di Active Directory sia almeno di livello 85.  Per eseguire l'aggiornamento di schema, il supporto di installazione con il Windows Server 2016, aprire un prompt dei comandi e passare alla directory support\adprep. Eseguire il comando seguente:  `adprep /forestprep`

    ![aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

    Una volta che viene completata l'esecuzione `adprep/domainprep`
    >[!NOTE]
    >Prima di eseguire il passaggio successivo, assicurarsi che Windows Server è corrente tramite l'esecuzione di Windows Update dalle impostazioni. Continua questo processo fino a quando non sono necessari altri aggiornamenti.
    >

    ![aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

8. Ora nel Server di Windows Server 2016 aprire PowerShell ed eseguire il cmdlet seguente:
    >[!NOTE]
    > Tutti i Server 2012 R2 deve essere rimosso dalla farm prima di eseguire il passaggio successivo.

    `Invoke-AdfsFarmBehaviorLevelRaise`  

    ![aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

9. Quando richiesto, digitare Y. Verrà avviata l'aumento del livello. Questo termine si sono generati correttamente il FBL.  

    ![aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

10. A questo punto, se si passa alla gestione di AD FS, verrà visualizzato che sono state aggiunte le nuove funzionalità per la versione più recente AD FS

    ![aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

11. Analogamente, è possibile usare il cmdlet di PowerShell: `Get-AdfsFarmInformation` per mostrarvi FBL corrente.  

    ![aggiornamento](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  

12. Per aggiornare i server WAP al livello più recente, in ogni Proxy applicazione Web, riconfigurare il WAP eseguendo il comando PowerShell seguente in una finestra con privilegi elevata:  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
    Rimuovere i vecchi server dal cluster e mantenere solo i server WAP in esecuzione la versione più recente di server, che sono stati riconfigurati sopra, eseguendo il commandlet di Powershell seguente.
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
    Controllare la configurazione di WAP eseguendo il commmandlet Get-WebApplicationProxyConfiguration. Il ConnectedServersName rifletterà server eseguita dal comando precedente.
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
    Per aggiornare la versione di configurazione dei server WAP, eseguire il comando Powershell seguente.
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
    Si completerà l'aggiornamento dei server WAP.
