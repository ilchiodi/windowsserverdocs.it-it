---
ms.assetid: 5e334c4e-75a7-453c-83e8-5ab4243cc685
title: Creare il primo server federativo in una server farm federativa
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 09b577ddcf722c6eac17ea145f29f9583d1cdb00
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408421"
---
# <a name="create-the-first-federation-server-in-a-federation-server-farm"></a>Creare il primo server federativo in una server farm federativa

Dopo aver installato il servizio ruolo Servizio federativo e configurato i certificati necessari in un computer, è possibile configurare il computer in modo che diventi un server federativo. È possibile utilizzare la procedura seguente per configurare il computer in modo che diventi il primo server federativo in una nuova Federazione server farm utilizzando la configurazione guidata server federativo di AD FS.  
  
Con la creazione del primo server federativo di una farm viene anche creato un nuovo Servizio federativo e il computer diventa il server federativo primario. Questo significa che il computer verrà configurato con una copia Read @ no__t-0write del database di configurazione AD FS. Tutti gli altri server federativi della farm devono replicare le modifiche apportate nel server federativo primario alle relative copie Read @ no__t-0only del database di configurazione AD FS archiviate localmente. Per altre informazioni sul processo di replica, vedere [Ruolo del database di configurazione di ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
> [!NOTE]  
> Per il Web single federato @ no__t-0Sign @ no__t-1Sulla Barra \(SSO @ no__t-3 progettazione, è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse. Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
Il requisito minimo per completare la procedura è l'appartenenza al gruppo di amministratori di dominio o a un account di dominio delegato a cui sia stato concesso l'accesso in scrittura al contenitore di dati di programma in Active Directory.  
  
### <a name="to-create-the-first-federation-server-in-a-federation-server-farm"></a>Per creare il primo server federativo in una server farm federativa.  
  
1.  Esistono due modi per avviare la configurazione guidata del server federativo di AD FS. Per avviare la procedura guidata, eseguire una delle operazioni seguenti:  
  
    -   Al termine dell'installazione del servizio ruolo Servizio federativo, aprire lo snap-in di gestione AD FS @ no__t-0cm e fare clic sul collegamento **ad FS configurazione guidata server federativo** nella pagina **Panoramica** o nel riquadro **azioni** .  
  
    -   In qualsiasi momento dopo il completamento dell'installazione guidata, aprire Esplora risorse, passare alla cartella **C: \\Windows @ no__t-2ADFS** e quindi a Double @ no__t-3Cliccare **FsConfigWizard. exe**.  
  
2.  Nella **pagina iniziale** verificare che sia selezionata l'opzione per **creare un nuovo servizio federativo**, quindi fare clic su **Avanti**.  
  
3.  Nella pagina **Select stand @ no__t-1Alone o farm Deployment** fare clic su **New Federation Server farm**, quindi fare clic su **Next**.  
  
4.  Nella pagina **Specifica il nome del servizio federativo** verificare che il **certificato SSL** visualizzato sia corretto. In caso contrario, selezionare il certificato appropriato nell'elenco di **certificati SSL**.  
  
    Questo certificato viene generato dalle impostazioni Secure Sockets Layer \(SSL @ no__t-1 per il sito Web predefinito. Se per il sito Web predefinito è configurato un unico certificato SSL, quest'ultimo viene presentato e selezionato automaticamente per l'uso. Se invece sono configurati più certificati SSL, vengono elencati tutti ed è necessario effettuare una selezione. Se non sono configurate impostazioni SSL per il sito Web predefinito, l'elenco viene generato dai certificati disponibili nell'archivio certificati personali del computer locale.  
  
    > [!NOTE]  
    > La procedura guidata non consente di ignorare il certificato se per IIS è configurato un certificato SSL. Ciò assicura che venga preservata qualsiasi configurazione IIS precedente per i certificati SSL. Per aggirare questa restrizione, è possibile rimuovere il certificato o riconfigurarlo manualmente nella Console di gestione IIS.  
  
5.  Se il database di AD FS selezionato esiste già, viene visualizzata la pagina **rilevato database di configurazione ad FS esistente** . In questo caso, fare clic su **Elimina database** e quindi su **Avanti**.  
  
    > [!CAUTION]  
    > Selezionare questa opzione solo quando si è certi che i dati in questo database di AD FS non sono importanti o che non vengono usati in una Federazione di produzione server farm.  
  
6.  Nella pagina **Specifica account del servizio** fare clic su **Sfoglia**. Nella finestra di dialogo **Sfoglia** individuare l'account di dominio che verrà usato come account del servizio nella nuova server farm federativa, quindi fare clic su **OK**. Immettere la password per l'account, confermarla e quindi fare clic su **Avanti**.  
  
    > [!NOTE]  
    > Vedere [configurare manualmente un account del servizio per una server farm federativa](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) per ulteriori informazioni sulla specifica di un account di servizio per una Federazione server farm. Ogni server federativo del server farm di federazione deve specificare lo stesso account del servizio per la farm come operativa. Se, ad esempio, l'account del servizio creato era contoso @ no__t-0ADFS2SVC, ogni computer configurato per il ruolo server federativo e che parteciperà alla stessa farm dovrà specificare contoso @ no__t-1ADFS2SVC in questo passaggio del server federativo. Configurazione guidata per la farm come operativa.  
  
7.  Esaminare i dettagli nella pagina **Applicazione delle impostazioni**. Se le impostazioni sembrano corrette, fare clic su **Avanti** per iniziare la configurazione di ad FS con queste impostazioni.  
  
8.  Esaminare i risultati nella pagina **Risultati di configurazione**. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
    > [!IMPORTANT]  
    > Per motivi di sicurezza della distribuzione, le opzioni per la risoluzione di elementi e il rilevamento delle risposte sono disabilitate quando si usa la Configurazione guidata server federativo ADFS per configurare una server farm federativa. La procedura guidata configura automaticamente il database interno di Windows per l'archiviazione dei dati di configurazione del servizio. È possibile, tuttavia, annullare erroneamente questa modifica abilitando l'endpoint di risoluzione artefatto usando il nodo **endpoint** nello snap-in di gestione ad FS @ no__t-1in o il cmdlet Enable @ No__t-2ADFSEndpoint in Windows PowerShell. Prestare attenzione a non riconfigurare l'impostazione predefinita in modo che questo endpoint rimanga disabilitato quando si usa una server farm federativa insieme al database interno di Windows.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

