---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Creare un server federativo autonomo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3a948fadeb1cfa2ebe259a9bb659e93a85e06910
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359664"
---
# <a name="create-a-stand-alone-federation-server"></a>Creare un server federativo autonomo

Dopo aver installato il servizio ruolo Servizio federativo e configurato i certificati necessari in un computer, è possibile configurare il computer in modo che diventi un server federativo. È possibile utilizzare la procedura seguente per configurare il computer in modo che diventi un server federativo autonomo\-. L'azione di creazione di un server federativo autonomo\-crea anche un nuovo Servizio federativo. Si crea un server federativo con la configurazione guidata server federativo di AD FS.  
  
> [!NOTE]  
> Per l'accesso Single Sign-on Web\-\-nella progettazione di \(SSO\) è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse. Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere [gruppi predefiniti locali e di dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Per creare un server federativo autonomo\-  
  
1.  Esistono due modi per avviare la configurazione guidata del server federativo di AD FS. Per avviare la procedura guidata, eseguire una delle operazioni seguenti:  
  
    -   Al termine dell'installazione del servizio ruolo Servizio federativo, aprire lo snap-in di gestione AD FS\-in e fare clic sul collegamento **ad FS la configurazione guidata del server federativo** nella pagina **Panoramica** o nel riquadro **azioni** .  
  
    -   In qualsiasi momento dopo aver completato l'installazione guidata, aprire Esplora risorse, passare alla cartella **C:\\windows\\ADFS** , quindi fare\-doppio clic su **FsConfigWizard. exe**.  
  
2.  Nella pagina inizialeverificare che sia selezionata l'opzione per creare un nuovo servizio federativo, quindi fare clic su **Avanti**.  
  
3.  Nella pagina **seleziona\-autonomo o distribuzione farm** fare clic su **server federativo autonomo\-solo**e quindi fare clic su **Avanti**.  
  
    > [!IMPORTANT]  
    > Quando si seleziona l'opzione server federativo autonomo\-in AD FS configurazione guidata server federativo, l'account del servizio associato a questo Servizio federativo verrà automaticamente assegnato all'account servizio di rete. È consigliabile utilizzare servizio di rete come account del servizio solo nelle situazioni in cui si sta valutando AD FS in un ambiente lab di test. Se si intende utilizzare l'opzione del server federativo stand\-alone per distribuire un server federativo in un ambiente di produzione, è importante modificare questo account del servizio in un account di servizio più appropriato che può essere dedicato per soddisfare le richieste per questo nuovo Servizio federativo. La modifica dell'account del servizio in un account diverso da servizio di rete mitiga i possibili vettori di attacco che altrimenti renderebbe il server federativo vulnerabile ad attacchi dannosi.  
  
4.  Nella pagina **Specifica il nome del servizio federativo** verificare che il **certificato SSL** visualizzato sia corretto. In caso contrario, selezionare il certificato appropriato dall'elenco **certificato SSL** .  
  
    Questo certificato viene generato dalla Secure Sockets Layer \(impostazioni\) SSL per il sito Web predefinito. Se per il sito Web predefinito è configurato un unico certificato SSL, quest'ultimo viene presentato e selezionato automaticamente per l'uso. Se invece sono configurati più certificati SSL, vengono elencati tutti ed è necessario effettuare una selezione. Se non sono configurate impostazioni SSL per il sito Web predefinito, l'elenco viene generato dai certificati disponibili nell'archivio certificati personali del computer locale.  
  
    > [!NOTE]  
    > La procedura guidata non consente di ignorare il certificato se per IIS è configurato un certificato SSL. Ciò assicura che venga preservata qualsiasi configurazione IIS precedente per i certificati SSL. Per ovviare a questa restrizione, è possibile rimuovere il certificato o riconfigurarlo manualmente con la console di gestione IIS.  
  
5.  Se il database di AD FS selezionato esiste già, viene visualizzata la pagina **rilevato database di configurazione ad FS esistente** . Se ciò accade, fare clic su **Elimina database**, quindi su **Avanti**.  
  
    > [!CAUTION]  
    > Selezionare questa opzione solo quando si è certi che i dati in questo database di AD FS non sono importanti o che non vengono usati in una Federazione di produzione server farm.  
  
6.  Esaminare i dettagli nella pagina **Applicazione delle impostazioni**. Se le impostazioni sembrano corrette, fare clic su **Avanti** per iniziare la configurazione di ad FS con queste impostazioni.  
  
7.  Esaminare i risultati nella pagina **Risultati di configurazione**. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

