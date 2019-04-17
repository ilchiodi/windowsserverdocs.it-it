---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Aggiungere un Server federativo a una Server Farm federativa
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d67f4c252ad25a05f11b88771f12fd01d13137d4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Aggiungere un Server federativo a una Server Farm federativa

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo avere installato il servizio ruolo servizio federativo e configurare i certificati necessari in un computer, si è pronti a configurare il computer come server federativo. È possibile utilizzare la procedura seguente per aggiungere un computer a una nuova server farm federativa.  
  
Aggiungere un computer a una farm con configurazione guidata Server federativo AD FS. Quando si utilizza questa procedura guidata per aggiungere un computer a una farm esistente, il computer è configurato con una sola copia del database di configurazione di ADFS e deve ricevere gli aggiornamenti da un server federativo primario.  
  
> [!NOTE]  
> Per la progettazione \(SSO\) Single\-Sign\-On Web federata, è necessario disporre di almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse. Per ulteriori informazioni, vedere [dove posizionare un Server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Per aggiungere un server federativo a una server farm federativa  
  
1.  Esistono due modi per avviare Configurazione guidata Server federativo AD FS. Per avviare la procedura guidata, effettuare una delle operazioni seguenti:  
  
    -   Una volta completata l'installazione del servizio ruolo servizio federativo, aprire Gestione di AD FS snap-in e fare clic sul **configurazione guidata Server federativo di AD FS** collegamento nel **Panoramica** pagina oppure il **azioni** riquadro.  
  
    -   In qualsiasi momento dopo l'installazione guidata, aprire Esplora risorse, passare al **C:\\Windows\\ADFS** cartella e fare doppio clic **FsConfigWizard.exe**.  
  
2.  Nel **iniziale** verificare che **aggiungere un server federativo a un servizio federativo esistente** sia selezionata e quindi fare clic su **Avanti**.  
  
3.  Se il database di ADFS selezionato già esiste, il **esistente AD FS rilevato Database di configurazione** verrà visualizzata la pagina. In questo caso, fare clic su **eliminare database**, quindi fare clic su **Avanti**.  
  
    > [!CAUTION]  
    > Selezionare questa opzione solo quando si è certi che i dati in questo database di ADFS non siano importanti o che non viene utilizzato in una server farm federativa di produzione.  
  
4.  Nel **specificare il Server federativo primario e un Account del servizio** nella pagina **nome del server federativo primario**, digitare il nome del computer del server federativo primario della farm e quindi fare clic su **Sfoglia**. Nel **Sfoglia** la finestra di dialogo, individuare l'account di dominio utilizzato come account del servizio da tutti gli altri server federativo nella server farm federativa esistente e quindi fare clic su **OK**. Digitare la password e confermarla e quindi fare clic su **Avanti**:  
  
    > [!NOTE]  
    > Per ulteriori informazioni su come specificare un account di servizio per una server farm federativa, vedere [configurare manualmente un Account del servizio per una Server Farm federativa](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Ogni server federativo nella server farm federativa è necessario specificare lo stesso account di servizio per la farm sia operativa. Ad esempio, se l'account del servizio creato era contoso\\ADFS2SVC, ogni computer configurato per il ruolo server federativo e che faranno parte della stessa farm è necessario specificare contoso\\ADFS2SVC in questo passaggio nella configurazione guidata Server federativo per la farm sia operativa.  
  
5.  Nel **applicazione delle impostazioni** pagina, esaminare i dettagli. Se le impostazioni sembrano corrette, fare clic su **Avanti** per iniziare a configurare ADFS con queste impostazioni.  
  
6.  Nel **risultati di configurazione** pagina, esaminare i risultati. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

