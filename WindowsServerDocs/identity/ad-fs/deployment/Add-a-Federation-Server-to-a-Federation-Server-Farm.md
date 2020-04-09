---
ms.assetid: 6ecf8d85-cd61-4c87-add8-00a679a6e3ff
title: Aggiungere un server federativo a una server farm federativa
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ea826f8984937c5d32a830131f042a8519528268
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815054"
---
# <a name="add-a-federation-server-to-a-federation-server-farm"></a>Aggiungere un server federativo a una server farm federativa


Dopo aver installato il servizio ruolo Servizio federativo e configurato i certificati necessari in un computer, è possibile configurare il computer in modo che diventi un server federativo. Per connettere un computer a una nuova server farm federativa, utilizzare la procedura seguente.  
  
È possibile aggiungere un computer a una farm mediante la configurazione guidata del server federativo ADFS. Quando si utilizza questa procedura guidata per aggiungere un computer a una farm esistente, il computer viene configurato con una copia\-sola lettura del database di configurazione AD FS e deve ricevere gli aggiornamenti da un server federativo primario.  
  
> [!NOTE]  
> Per l'accesso Single Sign-on Web\-\-nella progettazione di \(SSO\) è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse. Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere [gruppi predefiniti locali e di dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-add-a-federation-server-to-a-federation-server-farm"></a>Per aggiungere un server federativo a una Federazione server farm  
  
1.  Sono due i modi per avviare la configurazione guidata del server federativo di ADFS. Per avviare la procedura guidata, procedere in uno dei seguenti modi:  
  
    -   Al termine dell'installazione del servizio ruolo Servizio federativo, aprire lo snap-in di gestione AD FS\-in e fare clic sul collegamento **ad FS la configurazione guidata del server federativo** nella pagina **Panoramica** o nel riquadro **azioni** .  
  
    -   In qualsiasi momento dopo aver completato l'installazione guidata, aprire Esplora risorse, passare alla cartella **C:\\windows\\ADFS** e\-fare doppio clic su **FsConfigWizard. exe**.  
  
2.  Nella pagina **Welcome** verificare che sia selezionata l'opzione **Add a federation server to an existing Federation Service**, quindi fare clic su **Next**.  
  
3.  Se il database di AD FS selezionato esiste già, viene visualizzata la pagina **rilevato database di configurazione ad FS esistente** . In tal caso, fare clic su **Delete database**, quindi su **Next**.  
  
    > [!CAUTION]  
    > Selezionare tale opzione solo se si è certi che i dati contenuti in questo database di ADFS non siano importanti o non vengano usati in una server farm federativa di produzione.  
  
4.  Nella pagina **Specify the Primary Federation Server and Service Account**, in **Primary federation server name**, digitare il nome computer del server federativo primario della farm, quindi fare clic su **Browse**. Nella finestra di dialogo **Browse** individuare l'account di dominio usato come account di servizio da tutti gli altri server federativi della server farm federativa esistente, quindi fare clic su **OK**. Digitare la password e confermarla, quindi fare clic su **Next**.  
  
    > [!NOTE]  
    > Per ulteriori informazioni sulla specifica di un account di servizio per una server farm federativa, vedere [configurare manualmente un account del servizio per una server farm federativa](Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md). Ogni server federativo del server farm di federazione deve specificare lo stesso account del servizio per la farm come operativa. Se, ad esempio, l'account del servizio creato era contoso\\ADFS2SVC, ogni computer configurato per il ruolo server federativo e che parteciperà alla medesima farm dovrà specificare contoso\\ADFS2SVC in questo passaggio della configurazione guidata del server federativo affinché la farm sia operativa.  
  
5.  Nella pagina **Ready to Apply Settings** esaminare i dettagli. Se le impostazioni risultano corrette, fare clic su **Next** per avviare la configurazione di ADFS con queste impostazioni.  
  
6.  Nella pagina **Configuration Results** esaminare i risultati. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

