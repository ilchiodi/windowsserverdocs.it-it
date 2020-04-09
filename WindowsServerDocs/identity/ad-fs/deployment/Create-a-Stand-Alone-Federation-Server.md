---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Creare un server federativo autonomo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 603786d8553cce20f0b559ba8a91dfc29f760488
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855484"
---
# <a name="create-a-stand-alone-federation-server"></a>Creare un server federativo autonomo

Dopo aver installato il servizio ruolo Servizio federativo e configurato i certificati necessari in un computer, è possibile configurare il computer in modo che diventi un server federativo. È possibile utilizzare la procedura seguente per configurare il computer in modo che diventi un server federativo autonomo\-. L'azione di creazione di un server federativo autonomo\-crea anche un nuovo Servizio federativo. Si crea un server federativo con la configurazione guidata server federativo di AD FS.  
  
> [!NOTE]  
> Per l'accesso Single Sign-on Web\-\-nella progettazione di \(SSO\) è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse. Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
L'appartenenza al gruppo **Administrators**, o a un gruppo equivalente, nel computer locale è il requisito minimo per eseguire questa procedura.  Per informazioni dettagliate sull'uso degli account appropriati e delle appartenenze a gruppi, vedere [gruppi predefiniti locali e di dominio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Per creare un server federativo autonomo\-  
  
1.  Sono due i modi per avviare la configurazione guidata del server federativo di ADFS. Per avviare la procedura guidata, procedere in uno dei seguenti modi:  
  
    -   Al termine dell'installazione del servizio ruolo Servizio federativo, aprire lo snap-in di gestione AD FS\-in e fare clic sul collegamento **ad FS la configurazione guidata del server federativo** nella pagina **Panoramica** o nel riquadro **azioni** .  
  
    -   In qualsiasi momento dopo aver completato l'installazione guidata, aprire Esplora risorse, passare alla cartella **C:\\windows\\ADFS** , quindi fare\-doppio clic su **FsConfigWizard. exe**.  
  
2.  Nella pagina **Welcome** verificare che sia selezionata l'opzione **Create a new Federation Service**, quindi fare clic su **Next**.  
  
3.  Nella pagina **seleziona\-autonomo o distribuzione farm** fare clic su **server federativo autonomo\-solo**e quindi fare clic su **Avanti**.  
  
    > [!IMPORTANT]  
    > Quando si seleziona l'opzione server federativo autonomo\-in AD FS configurazione guidata server federativo, l'account del servizio associato a questo Servizio federativo verrà automaticamente assegnato all'account servizio di rete. È consigliabile utilizzare servizio di rete come account del servizio solo nelle situazioni in cui si sta valutando AD FS in un ambiente lab di test. Se si intende utilizzare l'opzione del server federativo stand\-alone per distribuire un server federativo in un ambiente di produzione, è importante modificare questo account del servizio in un account di servizio più appropriato che può essere dedicato per soddisfare le richieste per questo nuovo Servizio federativo. La modifica dell'account del servizio in un account diverso da servizio di rete mitiga i possibili vettori di attacco che altrimenti renderebbe il server federativo vulnerabile ad attacchi dannosi.  
  
4.  Nella pagina **Specify the Federation Service Name** verificare che l'impostazione di **SSL certificate** sia corretta. In caso contrario, selezionare il certificato appropriato dall'elenco **certificato SSL** .  
  
    Questo certificato viene generato dalla Secure Sockets Layer \(impostazioni\) SSL per il sito Web predefinito. Se in Sito Web predefinito è configurato un solo certificato SSL, questo verrà visualizzato e selezionato automaticamente per l'uso. Se per Sito Web predefinito sono configurati più certificati SSL, verranno elencati tutti e sarà necessario selezionarne uno. Se non sono configurate impostazioni SSL per Sito Web predefinito, l'elenco verrà generato in base ai certificati disponibili nell'archivio certificati personale nel computer locale.  
  
    > [!NOTE]  
    > La procedura guidata non consentirà di sovrascrivere il certificato se è stato configurato un certificato SSL per IIS. Ciò assicura la conservazione di qualsiasi configurazione destinata ai certificati SSL precedente a IIS. Per ovviare a questa restrizione, è possibile rimuovere il certificato o riconfigurarlo manualmente con la console di gestione IIS.  
  
5.  Se il database di ADFS selezionato esiste già, viene visualizzata la pagina **Existing AD FS Configuration Database Detected**. In tal caso, fare clic su **Delete database**, quindi su **Next**.  
  
    > [!CAUTION]  
    > Selezionare tale opzione solo se si è certi che i dati contenuti in questo database di ADFS non siano importanti o non vengano usati in una server farm federativa di produzione.  
  
6.  Nella pagina **Ready to Apply Settings** esaminare i dettagli. Se le impostazioni risultano corrette, fare clic su **Next** per avviare la configurazione di ADFS con queste impostazioni.  
  
7.  Nella pagina **Configuration Results** esaminare i risultati. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: configurazione di un server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

