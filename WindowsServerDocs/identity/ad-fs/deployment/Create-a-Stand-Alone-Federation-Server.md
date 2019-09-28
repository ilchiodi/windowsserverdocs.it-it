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

Dopo aver installato il servizio ruolo Servizio federativo e configurato i certificati necessari in un computer, è possibile configurare il computer in modo che diventi un server federativo. È possibile utilizzare la procedura seguente per configurare il computer in modo che diventi un server federativo stand @ no__t-0alone. L'atto di creare un server federativo stand @ no__t-0alone crea anche un nuovo Servizio federativo. Si crea un server federativo con la configurazione guidata server federativo di AD FS.  
  
> [!NOTE]  
> Per il Web single federato @ no__t-0Sign @ no__t-1Sulla Barra \(SSO @ no__t-3 progettazione, è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse. Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere \( [gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) di\/dominio http:\/\/go.Microsoft.com\/fwlink? LinkId\=83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Per creare un server federativo stand @ no__t-0alone  
  
1.  Esistono due modi per avviare la configurazione guidata del server federativo di AD FS. Per avviare la procedura guidata, eseguire una delle operazioni seguenti:  
  
    -   Al termine dell'installazione del servizio ruolo Servizio federativo, aprire lo snap-in di gestione AD FS @ no__t-0cm e fare clic sul collegamento **ad FS configurazione guidata server federativo** nella pagina **Panoramica** o nel riquadro **azioni** .  
  
    -   In qualsiasi momento dopo aver completato l'installazione guidata, aprire Esplora risorse, passare alla cartella **C: \\Windows @ no__t-2ADFS** e quindi a Double @ no__t-3Cliccare **FsConfigWizard. exe**.  
  
2.  Nella **pagina iniziale** verificare che sia selezionata l'opzione per **creare un nuovo servizio federativo**, quindi fare clic su **Avanti**.  
  
3.  Nella pagina **Select stand @ no__t-1Alone o farm Deployment** fare clic su **stand @ no__t-3alone Federation Server**, quindi fare clic su **Next**.  
  
    > [!IMPORTANT]  
    > Quando si seleziona l'opzione del server federativo stand @ no__t-0alone in AD FS configurazione guidata server federativo, l'account del servizio associato a questo Servizio federativo verrà automaticamente assegnato all'account servizio di rete. È consigliabile utilizzare servizio di rete come account del servizio solo nelle situazioni in cui si sta valutando AD FS in un ambiente lab di test. Se si intende utilizzare l'opzione server federativo stand @ no__t-0alone per distribuire un server federativo in un ambiente di produzione, è importante modificare questo account del servizio in un account del servizio più appropriato che può essere dedicato per soddisfare le richieste di Questo nuovo Servizio federativo. La modifica dell'account del servizio in un account diverso da servizio di rete mitiga i possibili vettori di attacco che altrimenti renderebbe il server federativo vulnerabile ad attacchi dannosi.  
  
4.  Nella pagina **Specifica il nome del servizio federativo** verificare che il **certificato SSL** visualizzato sia corretto. In caso contrario, selezionare il certificato appropriato dall'elenco **certificato SSL** .  
  
    Questo certificato viene generato dalle impostazioni Secure Sockets Layer \(SSL @ no__t-1 per il sito Web predefinito. Se per il sito Web predefinito è configurato un unico certificato SSL, quest'ultimo viene presentato e selezionato automaticamente per l'uso. Se invece sono configurati più certificati SSL, vengono elencati tutti ed è necessario effettuare una selezione. Se non sono configurate impostazioni SSL per il sito Web predefinito, l'elenco viene generato dai certificati disponibili nell'archivio certificati personali del computer locale.  
  
    > [!NOTE]  
    > La procedura guidata non consente di ignorare il certificato se per IIS è configurato un certificato SSL. Ciò assicura che venga preservata qualsiasi configurazione IIS precedente per i certificati SSL. Per ovviare a questa restrizione, è possibile rimuovere il certificato o riconfigurarlo manualmente con la console di gestione IIS.  
  
5.  Se il database di AD FS selezionato esiste già, viene visualizzata la pagina **rilevato database di configurazione ad FS esistente** . Se ciò accade, fare clic su **Elimina database**, quindi su **Avanti**.  
  
    > [!CAUTION]  
    > Selezionare questa opzione solo quando si è certi che i dati in questo database di AD FS non sono importanti o che non vengono usati in una Federazione di produzione server farm.  
  
6.  Esaminare i dettagli nella pagina **Applicazione delle impostazioni**. Se le impostazioni sembrano corrette, fare clic su **Avanti** per iniziare la configurazione di ad FS con queste impostazioni.  
  
7.  Esaminare i risultati nella pagina **Risultati di configurazione**. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

