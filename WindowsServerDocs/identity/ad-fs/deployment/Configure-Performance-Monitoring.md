---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: Configurare il monitoraggio delle prestazioni
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 37dd52b8771eda695069dd996fbd920e31f80ef1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359813"
---
# <a name="configure-performance-monitoring"></a>Configurare il monitoraggio delle prestazioni
  
## <a name="bkmk_ConfigurePerfMon"></a>  
AD FS include contatori delle prestazioni dedicati che consentono di monitorare le prestazioni dei computer server federativi e proxy server federativi. Per utilizzare Performance Monitor per monitorare le prestazioni dei server di AD FS, è utile creare un nuovo insieme agenti di raccolta dati e aggiungere i AD FS contatori a tale visualizzazione. Nella procedura seguente viene descritto come configurare il monitoraggio delle prestazioni per AD FS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Per configurare il monitoraggio delle prestazioni per AD FS tramite Performance Monitor  
  
1. Nella schermata **Start** digitare **Performance Monitor**e quindi premere INVIO.  
  
2. Nell'albero della console espandere insiemi agenti di **raccolta dati**, fare clic con il pulsante\-destro del mouse su **definito dall'utente**, scegliere **nuovo**, quindi fare clic su insieme agenti di **raccolta dati**.  
  
   Verrà visualizzata la procedura guidata Crea nuovo insieme agenti di raccolta dati.  
  
3. In **Crea nuovo insieme agenti di raccolta dati**, per **nome** digitare un nome per il nuovo insieme agenti di raccolta dati \(ad esempio "ad FS prestazioni"\), fare clic su **crea manualmente \(avanzate\)** , quindi fare clic su **Avanti**.  
  
4. Per il tipo di dati da includere, verificare che sia selezionata l'opzione **crea log dati** , quindi fare clic sulle caselle di controllo per i tipi di dati seguenti: **contatore delle prestazioni**, **dati di traccia eventi**, **informazioni sulla configurazione di sistema**.  
  
5. Per i contatori delle prestazioni, espandere **ad FS** nell'elenco **contatori disponibili** , quindi fare clic su **Aggiungi**.  
  
   I contatori delle prestazioni AD FS verranno visualizzati nell'elenco **contatori aggiunti** .  
  
6. Quando viene richiesto di aggiungere i provider di traccia eventi, fare clic su **Aggiungi**, selezionare **AD FS eventi** e **ad FS traccia** dall'elenco di provider.  
  
7. Quando viene richiesto di aggiungere le chiavi del registro di sistema da monitorare, fare clic su **Avanti**.  
  
8. Quando viene richiesto di specificare il percorso in cui salvare i dati sulle prestazioni, è possibile accettare il percorso predefinito \( **% SystemDrive%\\regprest\\Admin\\** _< data\_collector\_set >_ e quindi fare clic su **Avanti**.  
  
9. Quando viene richiesto di creare l'insieme agenti di raccolta dati, selezionare **Salva e Chiudi**, quindi fare clic su **fine**.  
  
    Il nuovo insieme agenti di raccolta dati viene visualizzato nell'albero della console sotto il nodo **definito dall'utente** .  
  
10. Per utilizzare i contatori delle prestazioni AD FS, attenersi alla procedura seguente:  
  
    -   Per avviare il monitoraggio delle prestazioni utilizzando AD FS\-contatori correlati, fare clic con il pulsante\-destro del mouse sull'insieme agenti di raccolta dati aggiunto \(ad esempio "ad FS performance", quindi fare clic su **Avvia**.\)  
  
    -   Per creare un report per visualizzare i risultati del monitoraggio delle prestazioni, fare clic con il pulsante\-destro del mouse sull'insieme agenti di raccolta dati aggiunti \(, ad esempio\)"AD FS performance", quindi fare clic su **report più recente**.  
  
    -   Per terminare un'acquisizione dei dati sulle prestazioni in modo che sia possibile visualizzare il report più recente, fare clic con il pulsante\-destro del mouse sull'insieme agenti di raccolta dati aggiunti \(, ad esempio\)"AD FS performance", quindi fare clic su **Arresta**.  
  
    Il report più recente viene aggiunto e numerato automaticamente \(a partire da 000001\) nel **report\\definito dall'utente** <em>\\< data\_collector\_set ></em> nodo nell'albero della console.  
  
## <a name="ad-fs-performance-counters"></a>Contatori delle prestazioni AD FS  
Nella tabella seguente sono elencati i contatori delle prestazioni AD FS e viene descritto il modo in cui sono utili per il monitoraggio dell'attività correlata a un server federativo o a un proxy server federativo.  
  
|Contatore|Descrizione|Può essere usato in: 
|-----------|---------------|------------------- 
|Richieste token|Esegue il monitoraggio del numero di richieste di token inviate al server federativo, incluse le richieste del token SSOAuth.|Server federativi 
|Richieste di token\/sec|Esegue il monitoraggio del numero di richieste di token inviate al server federativo, incluse le richieste di token SSOAuth al secondo.|Server federativi  
|Richieste metadati di federazione|Esegue il monitoraggio del numero di richieste di metadati federativi in ingresso inviate al server federativo.|Server federativi  
|Richieste dei metadati di Federazione\/sec|Esegue il monitoraggio del numero di richieste di metadati federativi in ingresso al secondo inviate al server federativo.|Server federativi  
|Richieste di risoluzione artefatto|Esegue il monitoraggio del numero di richieste di metadati federativi in ingresso al secondo inviate al server federativo.|Server federativi  
|Richieste di risoluzione artefatto\/sec|Esegue il monitoraggio del numero di richieste all'endpoint di risoluzione dell'artefatto al secondo inviate al server federativo.|Server federativi  
|Richieste proxy|Esegue il monitoraggio del numero di richieste in ingresso inviate al proxy server federativo.|Proxy server federativi  
|Richieste proxy\/sec|Esegue il monitoraggio del numero di richieste in ingresso al secondo inviate al proxy server federativo.|Proxy server federativi  
|Richieste MEX proxy|Esegue il monitoraggio del numero di richieste WS\-Metadata Exchange \(MEX\) inviate al proxy server federativo.|Proxy server federativi 
|Richieste MEX proxy\/sec|Esegue il monitoraggio del numero di richieste MEX in ingresso al secondo inviate al proxy server federativo.|Proxy server federativi  
  

