---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Aggiungere un server federativo a una server farm federativa
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 040caf6395b7c70313de900d522241f97699a999
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192499"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Aggiungere un server federativo a una server farm federativa


Dopo avere installato il servizio ruolo servizio federativo e configurare i certificati necessari in un computer, si è pronti a configurare il computer come un server federativo. Per connettere un computer a una nuova server farm federativa, utilizzare la procedura seguente.  
  
Si aggiunge un computer a una farm con configurazione guidata Server federativo AD FS. Quando si usa questa procedura guidata per aggiungere un computer a una farm esistente, il computer è configurato con un'operazione di lettura\-solo copia del database di configurazione di AD FS e si deve ricevere aggiornamenti da un server federativo primario.  
  
> [!NOTE]  
> Per il singolo Web federata\-Sign\-sul \(SSO\) progettazione, è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse . Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Per aggiungere un server federativo a una server farm federativa  
  
1.  Esistono due modi per avviare Configurazione guidata Server federativo AD FS. Per avviare la procedura guidata, eseguire una delle operazioni seguenti:  
  
    -   Una volta completata l'installazione del servizio ruolo servizio federativo, aprire lo snap di gestione di AD FS\-in e fare clic sui **configurazione guidata Server federativo di AD FS** sul collegamento il **Panoramica** pagina o nella **azioni** riquadro.  
  
    -   In qualsiasi momento dopo l'installazione guidata è completata, aprire Esplora Windows, passare al **c:\\Windows\\ADFS** cartella e fare doppio\-fare clic su **FsConfigWizard.exe**.  
  
2.  Nella pagina **Benvenuto**, verificare che l'opzione **Aggiunta di un server federativo a un servizio federativo esistente** sia selezionata, quindi fare clic su **Avanti**.  
  
3.  Se il database di AD FS selezionata sono già esiste, il **Existing AD FS Configuration Database Detected** verrà visualizzata la pagina. Se ciò accade, fare clic su **Elimina database**, quindi su **Avanti**.  
  
    > [!CAUTION]  
    > Selezionare questa opzione solo quando si è certi che i dati in questo database di ADFS non siano importanti o che non viene usato in una server farm federativa di produzione.  
  
4.  Nella pagina **Specificare il server primario federativo e l'account di servizio**, in **Nome server federativo primario**, digitare il nome computer del server federativo primario della farm, quindi fare clic su **Sfoglia**. Nella finestra di dialogo **Sfoglia**, individuare l'account di dominio che viene utilizzato come account di servizio da tutti gli altri server federativi della server farm federativa, quindi fare clic su **OK**. Digitare la password e confermarla e quindi fare clic su **successivo**:  
  
    > [!NOTE]  
    > Per altre informazioni su come specificare un account del servizio per una server farm federativa, vedere [configurare manualmente un Account del servizio per una Server Farm federativa](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Ogni server federativo nella server farm federativa è necessario specificare lo stesso account di servizio per la farm sia operativa. Ad esempio, se l'account del servizio creato era contoso\\ADFS2SVC, ogni computer viene configurato per il ruolo server federativo e che prenderà parte alla stessa farm devono specificare contoso\\ADFS2SVC in questo passaggio il Configurazione guidata Server federativo per la farm sia operativa.  
  
5.  Esaminare i dettagli nella pagina **Applicazione delle impostazioni**. Se le impostazioni sembrano corrette, fare clic su **successivo** per avviare la configurazione di ADFS con queste impostazioni.  
  
6.  Esaminare i risultati nella pagina **Risultati di configurazione**. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

