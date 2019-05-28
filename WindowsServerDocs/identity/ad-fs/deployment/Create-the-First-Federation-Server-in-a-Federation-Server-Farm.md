---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: Creare il primo server federativo in una server farm federativa
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: e16289142ea2e53adba52a4ed8f6c01a929a530d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192216"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Creare il primo server federativo in una server farm federativa

Dopo avere installato il servizio ruolo servizio federativo e configurare i certificati necessari in un computer, si è pronti a configurare il computer come un server federativo. È possibile utilizzare la procedura seguente per configurare il computer diventi il primo server federativo in una nuova server farm federativa utilizzando Configurazione guidata Server federativo AD FS.  
  
Con la creazione del primo server federativo di una farm viene anche creato un nuovo Servizio federativo e il computer diventa il server federativo primario. Ciò significa che il computer verrà configurato con un'operazione di lettura\/scrivere copia del database di configurazione di AD FS. Tutti gli altri server federativi della farm devono replicare le modifiche apportate nel server federativo primario per la lettura\-solo copie del database di configurazione di ADFS che memorizzano localmente. Per altre informazioni sul processo di replica, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Per il singolo Web federata\-Sign\-sul \(SSO\) progettazione, è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse . Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
Il requisito minimo per completare la procedura è l'appartenenza al gruppo di amministratori di dominio o a un account di dominio delegato a cui sia stato concesso l'accesso in scrittura al contenitore di dati di programma in Active Directory.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Per creare il primo server federativo in una server farm federativa.  
  
1.  Esistono due modi per avviare Configurazione guidata Server federativo AD FS. Per avviare la procedura guidata, eseguire una delle operazioni seguenti:  
  
    -   Una volta completata l'installazione del servizio ruolo servizio federativo, aprire lo snap di gestione di AD FS\-in e fare clic sui **configurazione guidata Server federativo di AD FS** sul collegamento il **Panoramica** pagina o nella **azioni** riquadro.  
  
    -   In qualsiasi momento dopo l'installazione guidata è completata, aprire Esplora Windows, passare al **c:\\Windows\\ADFS** cartella e quindi fare doppio\-fare clic su **FsConfigWizard.exe**.  
  
2.  Nella **pagina iniziale** verificare che sia selezionata l'opzione per **creare un nuovo servizio federativo**, quindi fare clic su **Avanti**.  
  
3.  Nel **seleziona in evidenza\-Alone o distribuzione della Farm** pagina, fare clic su **nuova server farm federativa**e quindi fare clic su **successivo**.  
  
4.  Nella pagina **Specifica il nome del servizio federativo** verificare che il **certificato SSL** visualizzato sia corretto. In caso contrario, selezionare il certificato appropriato nell'elenco di **certificati SSL**.  
  
    Questo certificato viene generato da Secure Sockets Layer \(SSL\) impostazioni per il sito Web predefinito. Se per il sito Web predefinito è configurato un unico certificato SSL, quest'ultimo viene presentato e selezionato automaticamente per l'uso. Se invece sono configurati più certificati SSL, vengono elencati tutti ed è necessario effettuare una selezione. Se non sono configurate impostazioni SSL per il sito Web predefinito, l'elenco viene generato dai certificati disponibili nell'archivio certificati personali del computer locale.  
  
    > [!NOTE]  
    > La procedura guidata non consente di ignorare il certificato se per IIS è configurato un certificato SSL. Ciò assicura che venga preservata qualsiasi configurazione IIS precedente per i certificati SSL. Per aggirare questa restrizione, è possibile rimuovere il certificato o riconfigurarlo manualmente nella Console di gestione IIS.  
  
5.  Se il database di AD FS selezionata sono già esiste, il **Existing AD FS Configuration Database Detected** verrà visualizzata la pagina. In questo caso, fare clic su **Elimina database** e quindi su **Avanti**.  
  
    > [!CAUTION]  
    > Selezionare questa opzione solo quando si è certi che i dati in questo database di ADFS non siano importanti o che non viene usato in una server farm federativa di produzione.  
  
6.  Nella pagina **Specifica account del servizio** fare clic su **Sfoglia**. Nella finestra di dialogo **Sfoglia** individuare l'account di dominio che verrà usato come account del servizio nella nuova server farm federativa, quindi fare clic su **OK**. Immettere la password per l'account, confermarla e quindi fare clic su **Avanti**.  
  
    > [!NOTE]  
    > Visualizzare [configurare manualmente un Account del servizio per una Server Farm federativa](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) per altre informazioni su come specificare un account del servizio per una server farm federativa. Ogni server federativo nella server farm federativa è necessario specificare lo stesso account di servizio per la farm sia operativa. Ad esempio, se l'account del servizio creato era contoso\\ADFS2SVC, ogni computer in cui viene configurato per il ruolo server federativo e che prenderà parte alla stessa farm devono specificare contoso\\ADFS2SVC in questo passaggio il Configurazione guidata Server federativo per la farm sia operativa.  
  
7.  Esaminare i dettagli nella pagina **Applicazione delle impostazioni**. Se le impostazioni sembrano corrette, fare clic su **successivo** per avviare la configurazione di ADFS con queste impostazioni.  
  
8.  Esaminare i risultati nella pagina **Risultati di configurazione**. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
    > [!IMPORTANT]  
    > Per motivi di sicurezza della distribuzione, le opzioni per la risoluzione di elementi e il rilevamento delle risposte sono disabilitate quando si usa la Configurazione guidata server federativo ADFS per configurare una server farm federativa. La procedura guidata configura automaticamente il database interno di Windows per l'archiviazione dei dati di configurazione del servizio. È potrebbero, tuttavia, erroneamente annullarle abilitando l'endpoint di risoluzione artefatto usando il **endpoint** nodo nello snap di gestione di AD FS\-in o consentire\-cmdlet ADFSEndpoint in Windows PowerShell. Prestare attenzione a non riconfigurare l'impostazione predefinita in modo che questo endpoint rimanga disabilitato quando si usa una server farm federativa insieme al database interno di Windows.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

