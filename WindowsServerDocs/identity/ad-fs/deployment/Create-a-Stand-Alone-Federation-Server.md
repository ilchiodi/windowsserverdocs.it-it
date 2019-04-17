---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Creare un Server federativo autonomo
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fd075c5b7d1bfce89cc27c4917a016e7e5037ce5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-stand-alone-federation-server"></a>Creare un Server federativo autonomo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo avere installato il servizio ruolo servizio federativo e configurare i certificati necessari in un computer, si è pronti a configurare il computer come server federativo. È possibile utilizzare la seguente procedura per configurare il computer a un server federativo autonoma. L'atto di creazione di un server federativo conseguono crea anche un nuovo servizio federativo. Creare un server federativo con configurazione guidata Server federativo AD FS.  
  
> [!NOTE]  
> Per la progettazione \(SSO\) Single\-Sign\-On Web federata, è necessario disporre di almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse. Per ulteriori informazioni, vedere [dove posizionare un Server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
Appartenenza al gruppo **amministratori**, o equivalente nel computer locale è il requisito minimo necessario per completare questa procedura.  Ulteriori informazioni sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Per creare un server federativo autonoma  
  
1.  Esistono due modi per avviare Configurazione guidata Server federativo AD FS. Per avviare la procedura guidata, effettuare una delle operazioni seguenti:  
  
    -   Una volta completata l'installazione del servizio ruolo servizio federativo, aprire Gestione di AD FS snap-in e fare clic sul **configurazione guidata Server federativo di AD FS** collegamento nel **Panoramica** pagina oppure il **azioni** riquadro.  
  
    -   In qualsiasi momento dopo l'installazione guidata, aprire Esplora risorse, passare al **C:\\Windows\\ADFS** cartella, quindi fare doppio clic **FsConfigWizard.exe**.  
  
2.  Nel **iniziale** verificare che **creare un nuovo servizio federativo** sia selezionata e quindi fare clic su **Avanti**.  
  
3.  Nel **selezionare autonoma o Farm** pagina, fare clic su **conseguono server federativo**, quindi fare clic su **Avanti**.  
  
    > [!IMPORTANT]  
    > Quando si seleziona l'opzione server federativo conseguono nella configurazione guidata Server federativo AD FS, l'account del servizio associato al servizio federativo verrà assegnato automaticamente all'account del servizio di rete. Utilizzo del servizio di rete come account del servizio è consigliata solo nelle situazioni in cui si sta valutando ADFS in un ambiente di testing. Se si intende utilizzare l'opzione server federativo autonoma per distribuire un server federativo in un ambiente di produzione, è importante che modificare questo account di servizio a un account di servizio più appropriato che possa essere dedicato all'elaborazione delle richieste per il nuovo servizio federativo. Modifica l'account del servizio a un account diverso da servizio di rete verrà ridurre i possibili vettori di attacco che sarebbero altrimenti rendere il server federativo vulnerabile ad attacchi dannosi.  
  
4.  Nel **specificare il nome del servizio federativo** verificare che il **certificato SSL** visualizzato sia corretto. In caso contrario, selezionare il certificato appropriato dal **certificato SSL** elenco.  
  
    Questo certificato viene generato dalle impostazioni \(SSL\) SSL (Secure Sockets Layer) per il sito Web predefinito. Se il sito Web predefinito ha un solo certificato SSL configurato, che il certificato viene presentato e selezionato automaticamente per l'uso. Se sono configurati più certificati SSL per il sito Web predefinito, è necessario effettuare una selezione di seguito sono elencati tutti i certificati. Se non sono presenti impostazioni SSL configurate per il sito Web predefinito, l'elenco viene generato dai certificati disponibili nell'archivio certificati personali del computer locale.  
  
    > [!NOTE]  
    > La procedura guidata non consente di ignorare il certificato se un certificato SSL è configurato per IIS. Ciò garantisce che qualsiasi destinata viene mantenuta la configurazione IIS precedente per i certificati SSL. Per ovviare a questa limitazione, è possibile rimuovere il certificato o riconfigurarlo manualmente con la Console di gestione IIS.  
  
5.  Se il database di ADFS selezionato già esiste, il **esistente AD FS rilevato Database di configurazione** verrà visualizzata la pagina. In questo caso, fare clic su **eliminare database**, quindi fare clic su **Avanti**.  
  
    > [!CAUTION]  
    > Selezionare questa opzione solo quando si è certi che i dati in questo database di ADFS non siano importanti o che non viene utilizzato in una server farm federativa di produzione.  
  
6.  Nel **applicazione delle impostazioni** pagina, esaminare i dettagli. Se le impostazioni sembrano corrette, fare clic su **Avanti** per iniziare a configurare ADFS con queste impostazioni.  
  
7.  Nel **risultati di configurazione** pagina, esaminare i risultati. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
## <a name="additional-references"></a>Riferimenti aggiuntivi  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

