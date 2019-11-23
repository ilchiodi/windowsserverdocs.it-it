---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Aggiungere un server federativo a una server farm federativa
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 57c59241c36ba4ed657b83e7c691931f57978a7c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408523"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Aggiungere un server federativo a una server farm federativa


Dopo aver installato il servizio ruolo Servizio federativo e configurato i certificati necessari in un computer, è possibile configurare il computer in modo che diventi un server federativo. Per connettere un computer a una nuova server farm federativa, utilizzare la procedura seguente.  
  
È possibile aggiungere un computer a una farm con la configurazione guidata server federativo di AD FS. Quando si utilizza questa procedura guidata per aggiungere un computer a una farm esistente, il computer viene configurato con una copia\-sola lettura del database di configurazione AD FS e deve ricevere gli aggiornamenti da un server federativo primario.  
  
> [!NOTE]  
> Per l'accesso Single Sign-on Web\-\-nella progettazione di \(SSO\) è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse. Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere [gruppi predefiniti locali e di dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Per aggiungere un server federativo a una Federazione server farm  
  
1.  Esistono due modi per avviare la configurazione guidata del server federativo di AD FS. Per avviare la procedura guidata, eseguire una delle operazioni seguenti:  
  
    -   Al termine dell'installazione del servizio ruolo Servizio federativo, aprire lo snap-in di gestione AD FS\-in e fare clic sul collegamento **ad FS la configurazione guidata del server federativo** nella pagina **Panoramica** o nel riquadro **azioni** .  
  
    -   In qualsiasi momento dopo aver completato l'installazione guidata, aprire Esplora risorse, passare alla cartella **C:\\windows\\ADFS** e\-fare doppio clic su **FsConfigWizard. exe**.  
  
2.  Nella pagina **Benvenuto**, verificare che l'opzione **Aggiunta di un server federativo a un servizio federativo esistente** sia selezionata, quindi fare clic su **Avanti**.  
  
3.  Se il database di AD FS selezionato esiste già, viene visualizzata la pagina **rilevato database di configurazione ad FS esistente** . Se ciò accade, fare clic su **Elimina database**, quindi su **Avanti**.  
  
    > [!CAUTION]  
    > Selezionare questa opzione solo quando si è certi che i dati in questo database di AD FS non sono importanti o che non vengono usati in una Federazione di produzione server farm.  
  
4.  Nella pagina **Specificare il server primario federativo e l'account di servizio**, in **Nome server federativo primario**, digitare il nome computer del server federativo primario della farm, quindi fare clic su **Sfoglia**. Nella finestra di dialogo **Sfoglia**, individuare l'account di dominio che viene utilizzato come account di servizio da tutti gli altri server federativi della server farm federativa, quindi fare clic su **OK**. Digitare la password e confermarla, quindi fare clic su **Avanti**:  
  
    > [!NOTE]  
    > Per ulteriori informazioni sulla specifica di un account di servizio per una server farm federativa, vedere [configurare manualmente un account del servizio per una server farm federativa](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Ogni server federativo del server farm di federazione deve specificare lo stesso account del servizio per la farm come operativa. Se, ad esempio, l'account del servizio creato era contoso\\ADFS2SVC, ogni computer configurato per il ruolo server federativo e che parteciperà alla medesima farm dovrà specificare contoso\\ADFS2SVC in questo passaggio della configurazione guidata del server federativo affinché la farm sia operativa.  
  
5.  Esaminare i dettagli nella pagina **Applicazione delle impostazioni**. Se le impostazioni sembrano corrette, fare clic su **Avanti** per iniziare la configurazione di ad FS con queste impostazioni.  
  
6.  Esaminare i risultati nella pagina **Risultati di configurazione**. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

