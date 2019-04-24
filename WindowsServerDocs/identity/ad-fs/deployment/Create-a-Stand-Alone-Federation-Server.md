---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Creare un server federativo autonomo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fd075c5b7d1bfce89cc27c4917a016e7e5037ce5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888292"
---
# <a name="create-a-stand-alone-federation-server"></a>Creare un server federativo autonomo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo avere installato il servizio ruolo servizio federativo e configurare i certificati necessari in un computer, si è pronti a configurare il computer come un server federativo. È possibile utilizzare la procedura seguente per configurare il computer come un supporto\-server federativo da solo. L'atto di creare un supporto\-server federativo da solo consente anche di creare un nuovo servizio federativo. Si crea un server federativo con configurazione guidata Server federativo AD FS.  
  
> [!NOTE]  
> Per il singolo Web federata\-Sign\-sul \(SSO\) progettazione, è necessario avere almeno un server federativo nell'organizzazione partner account e almeno un server federativo nell'organizzazione partner risorse . Per altre informazioni, vedere [Dove posizionare un server federativo](https://technet.microsoft.com/library/dd807127.aspx).  
  
Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Per creare un supporto\-server federativo da solo  
  
1.  Esistono due modi per avviare Configurazione guidata Server federativo AD FS. Per avviare la procedura guidata, eseguire una delle operazioni seguenti:  
  
    -   Una volta completata l'installazione del servizio ruolo servizio federativo, aprire lo snap di gestione di AD FS\-in e fare clic sui **configurazione guidata Server federativo di AD FS** sul collegamento il **Panoramica** pagina o nella **azioni** riquadro.  
  
    -   In qualsiasi momento dopo l'installazione guidata è completata, aprire Esplora Windows, passare al **c:\\Windows\\ADFS** cartella e quindi fare doppio\-fare clic su **FsConfigWizard.exe**.  
  
2.  Nella pagina iniziale **** verificare che sia selezionata l'opzione per creare un nuovo servizio federativo, quindi fare clic su **Avanti**.****  
  
3.  Nel **selezionare in evidenza\-Alone o distribuzione di Farm** pagina, fare clic su **Stand\-server federativo da solo**e quindi fare clic su **successivo**.  
  
    > [!IMPORTANT]  
    > Quando si seleziona il supporto\-opzione del server federativo autonomo nella configurazione guidata Server federativo AD FS, l'account del servizio associato al servizio federativo verrà automaticamente assegnato all'account del servizio di rete. Uso del servizio di rete come account del servizio è consigliata solo in situazioni in cui si sta valutando AD FS in un ambiente di testing. Se si prevede di usare il supporto per\-opzione del server federativo da solo per distribuire un server federativo in un ambiente di produzione, è importante modificare l'account del servizio in un account di servizio più appropriato che può essere dedicato alla gestione richieste per questo nuovo servizio federativo. Modifica l'account del servizio a un account diverso dal servizio di rete, è possibile limitare i vettori di attacco possibili che potrebbero rendere il server federativo vulnerabile ad attacchi dannosi.  
  
4.  Nella pagina **Specifica il nome del servizio federativo** verificare che il **certificato SSL** visualizzato sia corretto. In caso contrario, selezionare il certificato appropriato dal **certificato SSL** elenco.  
  
    Questo certificato viene generato da Secure Sockets Layer \(SSL\) impostazioni per il sito Web predefinito. Se per il sito Web predefinito è configurato un unico certificato SSL, quest'ultimo viene presentato e selezionato automaticamente per l'uso. Se invece sono configurati più certificati SSL, vengono elencati tutti ed è necessario effettuare una selezione. Se non sono configurate impostazioni SSL per il sito Web predefinito, l'elenco viene generato dai certificati disponibili nell'archivio certificati personali del computer locale.  
  
    > [!NOTE]  
    > La procedura guidata non consente di ignorare il certificato se per IIS è configurato un certificato SSL. Ciò assicura che venga preservata qualsiasi configurazione IIS precedente per i certificati SSL. Per aggirare questa limitazione, è possibile rimuovere il certificato o riconfigurarlo manualmente con la Console di gestione IIS.  
  
5.  Se il database di AD FS selezionata sono già esiste, il **Existing AD FS Configuration Database Detected** verrà visualizzata la pagina. Se ciò accade, fare clic su **Elimina database**, quindi su **Avanti**.  
  
    > [!CAUTION]  
    > Selezionare questa opzione solo quando si è certi che i dati in questo database di ADFS non siano importanti o che non viene usato in una server farm federativa di produzione.  
  
6.  Esaminare i dettagli nella pagina **Applicazione delle impostazioni**. Se le impostazioni sembrano corrette, fare clic su **successivo** per avviare la configurazione di ADFS con queste impostazioni.  
  
7.  Esaminare i risultati nella pagina **Risultati di configurazione**. Al termine di tutti i passaggi di configurazione, fare clic su **Chiudi** per uscire dalla procedura guidata.  
  
## <a name="additional-references"></a>Altri riferimenti  
[Elenco di controllo: Configurazione di un Server federativo](Checklist--Setting-Up-a-Federation-Server.md)  
  

