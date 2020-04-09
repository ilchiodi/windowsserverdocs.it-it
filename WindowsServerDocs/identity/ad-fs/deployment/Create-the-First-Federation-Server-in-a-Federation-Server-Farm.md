---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: Creare il primo server federativo in una server farm federativa
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0fe5c3160f661357536ef3bd60762873063c8ed0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855474"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Creare il primo server federativo in una server farm federativa

Dopo aver installato il servizio ruolo Servizio federativo e configurato i certificati necessari in un computer, è possibile configurare il computer in modo che diventi un server federativo. È possibile utilizzare la procedura seguente per configurare il computer in modo che diventi il primo server federativo in una nuova Federazione server farm utilizzando la configurazione guidata server federativo di AD FS.  
  
Con la creazione del primo server federativo di una farm viene anche creato un nuovo Servizio federativo e il computer diventa il server federativo primario. Questo significa che il computer verrà configurato con una copia di lettura\/scrittura del database di configurazione AD FS. Tutti gli altri server federativi della farm devono replicare tutte le modifiche apportate nel server federativo primario per la lettura\-solo le copie del database di configurazione AD FS archiviate localmente. Per altre informazioni sul processo di replica, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Per l'accesso Single Sign-on Web\-\-nella progettazione di \(SSO\) è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse. Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
Il requisito minimo per completare la procedura è l'appartenenza al gruppo di amministratori di dominio o a un account di dominio delegato a cui sia stato concesso l'accesso in scrittura al contenitore di dati di programma in Active Directory.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Per creare il primo server federativo in una server farm federativa.  
  
1.  Sono due i modi per avviare la configurazione guidata del server federativo di ADFS. Per avviare la procedura guidata, procedere in uno dei seguenti modi:  
  
    -   Al termine dell'installazione del servizio ruolo Servizio federativo, aprire lo snap-in di gestione AD FS\-in e fare clic sul collegamento **ad FS la configurazione guidata del server federativo** nella pagina **Panoramica** o nel riquadro **azioni** .  
  
    -   In qualsiasi momento dopo il completamento dell'installazione guidata, aprire Esplora risorse, passare alla cartella **C:\\windows\\ADFS** , quindi fare\-doppio clic su **FsConfigWizard. exe**.  
  
2.  Nella pagina **Welcome** verificare che sia selezionata l'opzione **Create a new Federation Service**, quindi fare clic su **Next**.  
  
3.  Nella pagina **seleziona\-autonomo o distribuzione farm** fare clic su **nuova Federazione server farm**, quindi fare clic su **Avanti**.  
  
4.  Nella pagina **Specify the Federation Service Name** verificare che l'impostazione di **SSL certificate** sia corretta. Se questo non è il certificato corretto, selezionare il certificato appropriato dall'elenco **SSL certificate**.  
  
    Questo certificato viene generato dalla Secure Sockets Layer \(impostazioni\) SSL per il sito Web predefinito. Se in Sito Web predefinito è configurato un solo certificato SSL, questo verrà visualizzato e selezionato automaticamente per l'uso. Se per Sito Web predefinito sono configurati più certificati SSL, verranno elencati tutti e sarà necessario selezionarne uno. Se non sono configurate impostazioni SSL per Sito Web predefinito, l'elenco verrà generato in base ai certificati disponibili nell'archivio certificati personale nel computer locale.  
  
    > [!NOTE]  
    > La procedura guidata non consentirà di sovrascrivere il certificato se è stato configurato un certificato SSL per IIS. Ciò assicura la conservazione di qualsiasi configurazione destinata ai certificati SSL precedente a IIS. Per ovviare a questo problema, è possibile rimuovere il certificato o riconfigurarlo manualmente con la Console di gestione IIS.  
  
5.  Se il database di ADFS selezionato esiste già, viene visualizzata la pagina **Existing AD FS Configuration Database Detected**. In tal caso fare clic su **Delete database**, quindi su **Next**.  
  
    > [!CAUTION]  
    > Selezionare tale opzione solo se si è certi che i dati contenuti in questo database di ADFS non siano importanti o non vengano usati in una server farm federativa di produzione.  
  
6.  Nella pagina **Specify a Service Account** fare clic su **Browse**. Nella finestra di dialogo **Browse** individuare l'account di dominio che verrà usato come account di servizio nella nuova server farm federativa, quindi fare clic su **OK**. Digitare la password per l'account e confermarla, quindi fare clic su **Next**.  
  
    > [!NOTE]  
    > Vedere [configurare manualmente un account del servizio per una server farm federativa](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) per ulteriori informazioni sulla specifica di un account di servizio per una Federazione server farm. Ogni server federativo del server farm di federazione deve specificare lo stesso account del servizio per la farm come operativa. Se, ad esempio, l'account del servizio creato era contoso\\ADFS2SVC, ogni computer configurato per il ruolo server federativo e che parteciperà alla medesima farm dovrà specificare contoso\\ADFS2SVC in questo passaggio della configurazione guidata del server federativo affinché la farm sia operativa.  
  
7.  Nella pagina **Ready to Apply Settings** esaminare i dettagli. Se le impostazioni risultano corrette, fare clic su **Next** per avviare la configurazione di ADFS con queste impostazioni.  
  
8.  Nella pagina **Configuration Results** esaminare i risultati. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
    > [!IMPORTANT]  
    > Per garantire una distribuzione sicura, la risoluzione degli artefatti e l'individuazione delle risposte sono disabilitate quando si usa la configurazione guidata del server federativo di ADFS per configurare una server farm federativa. Questa procedura guidata configura automaticamente Database interno di Windows per l'archiviazione dei dati di configurazione dei servizi. È possibile, tuttavia, annullare erroneamente questa modifica abilitando l'endpoint di risoluzione artefatto usando il nodo **endpoint** nello snap-in di gestione ad FS\-in o il cmdlet Enable\-ADFSEndpoint in Windows PowerShell. Evitare quindi di riconfigurare l'impostazione predefinita. In questo modo l'endpoint rimane disabilitato quando si usano insieme una server farm federativa e Database interno di Windows.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

