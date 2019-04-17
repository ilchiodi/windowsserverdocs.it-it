---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: Creare il primo Server federativo in una Server Farm federativa
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: af0aa61f0d16d4ca567b140c95d74445d09f1cf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Creare il primo Server federativo in una Server Farm federativa

 >Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo avere installato il servizio ruolo servizio federativo e configurare i certificati necessari in un computer, si è pronti a configurare il computer come server federativo. È possibile utilizzare la seguente procedura per configurare il computer per il primo server federativo in una nuova server farm federativa tramite configurazione guidata Server federativo AD FS.  
  
Inoltre, l'atto di creare il primo server federativo in una farm crea un nuovo servizio federativo e il computer diventa il server federativo primario. Ciò significa che il computer verrà configurato con una copia di lettura/scrittura del database di configurazione di ADFS. Tutti gli altri server federativi della farm devono replicare le modifiche apportate nel server federativo primario per le copie di sola lettura del database di configurazione di ADFS che memorizzano localmente. Per ulteriori informazioni sul processo di replica, vedere [il ruolo del Database di configurazione AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Per la progettazione \(SSO\) Single\-Sign\-On Web federata, è necessario disporre di almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse. Per ulteriori informazioni, vedere [dove posizionare un Server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
Appartenenza al gruppo Domain Admins oppure un account di dominio delegato a cui è stato concesso l'accesso in scrittura al contenitore di dati del programma in Active Directory, è il requisito minimo necessario per completare questa procedura.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Per creare il primo server federativo in una server farm federativa  
  
1.  Esistono due modi per avviare Configurazione guidata Server federativo AD FS. Per avviare la procedura guidata, effettuare una delle operazioni seguenti:  
  
    -   Una volta completata l'installazione del servizio ruolo servizio federativo, aprire Gestione di AD FS snap-in e fare clic sul **configurazione guidata Server federativo di AD FS** collegamento nel **Panoramica** pagina oppure il **azioni** riquadro.  
  
    -   In qualsiasi momento dopo l'installazione guidata di è completo, aprire Esplora risorse, passare al **C:\\Windows\\ADFS** cartella, quindi fare doppio clic **FsConfigWizard.exe**.  
  
2.  Nel **iniziale** verificare che **creare un nuovo servizio federativo** sia selezionata e quindi fare clic su **Avanti**.  
  
3.  Nel **selezionare autonoma o Farm** pagina, fare clic su **nuova server farm federativa**, quindi fare clic su **Avanti**.  
  
4.  Nel **specificare il nome del servizio federativo** verificare che il **certificato SSL** visualizzato sia corretto. Se non si tratta del certificato corretto, selezionare il certificato appropriato dal **certificato SSL** elenco.  
  
    Questo certificato viene generato dalle impostazioni \(SSL\) SSL (Secure Sockets Layer) per il sito Web predefinito. Se il sito Web predefinito ha un solo certificato SSL configurato, che il certificato viene presentato e selezionato automaticamente per l'uso. Se sono configurati più certificati SSL per il sito Web predefinito, è necessario effettuare una selezione di seguito sono elencati tutti i certificati. Se non sono presenti impostazioni SSL configurate per il sito Web predefinito, l'elenco viene generato dai certificati disponibili nell'archivio certificati personali del computer locale.  
  
    > [!NOTE]  
    > La procedura guidata non consente di ignorare il certificato se un certificato SSL è configurato per IIS. Ciò garantisce che qualsiasi destinata viene mantenuta la configurazione IIS precedente per i certificati SSL. Per ovviare a questa limitazione, è possibile rimuovere il certificato o riconfigurarlo manualmente la Console di gestione IIS.  
  
5.  Se il database di ADFS selezionato già esiste, il **esistente AD FS rilevato Database di configurazione** verrà visualizzata la pagina. Se viene visualizzata la pagina, fare clic su **eliminare database**, quindi fare clic su **Avanti**.  
  
    > [!CAUTION]  
    > Selezionare questa opzione solo quando si è certi che i dati in questo database di ADFS non siano importanti o che non viene utilizzato in una server farm federativa di produzione.  
  
6.  Nel **specificare un Account del servizio** pagina, fare clic su **Sfoglia**. Nel **Sfoglia** finestra di dialogo, individuare l'account di dominio che verrà utilizzato come account del servizio in questa nuova server farm federativa, quindi fare clic su **OK**. Digitare la password per questo account, confermarla e quindi fare clic su **Avanti**.  
  
    > [!NOTE]  
    > Vedere [configurare manualmente un Account del servizio per una Server Farm federativa](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) per ulteriori informazioni su come specificare un account di servizio per una server farm federativa. Ogni server federativo nella server farm federativa è necessario specificare lo stesso account di servizio per la farm sia operativa. Ad esempio, se l'account del servizio creato era contoso\\ADFS2SVC, ogni computer che si configura per il ruolo server federativo e che faranno parte della stessa farm è necessario specificare contoso\\ADFS2SVC in questo passaggio nella configurazione guidata Server federativo per la farm sia operativa.  
  
7.  Nel **applicazione delle impostazioni** pagina, esaminare i dettagli. Se le impostazioni sembrano corrette, fare clic su **Avanti** per iniziare a configurare ADFS con queste impostazioni.  
  
8.  Nel **risultati di configurazione** pagina, esaminare i risultati. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
    > [!IMPORTANT]  
    > Per motivi di sicurezza della distribuzione, il rilevamento di risposta e di risoluzione artefatto sono disabilitati quando si utilizza configurazione guidata Server federativo AD FS per configurare una server farm federativa. Questa procedura guidata Configura automaticamente il Database interno di Windows per l'archiviazione dei dati di configurazione del servizio. Questa modifica potrebbe, tuttavia, annullare erroneamente abilitando l'endpoint di risoluzione artefatto utilizzando il **endpoint** nodo snap-in di gestione di ADFS o il cmdlet Enable-ADFSEndpoint di Windows PowerShell. Prestare attenzione a non riconfigurare l'impostazione predefinita, in modo che questo endpoint rimanga disabilitato quando si utilizza una server farm federativa con Database interno di Windows insieme.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

