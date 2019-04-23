---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: Configurare il monitoraggio delle prestazioni
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6a7602cddcaee274d42213cd9365f6d1722dab79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852912"
---
# <a name="configure-performance-monitoring"></a>Configurare il monitoraggio delle prestazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="bkmk_ConfigurePerfMon"></a>  
ADFS include un proprio i contatori delle prestazioni dedicati per monitorare le prestazioni di entrambi i server federativi e computer proxy server federativo. Per utilizzare Performance Monitor per monitorare le prestazioni dei server AD FS, è utile creare un nuovo insieme agenti di raccolta dati e aggiungere i contatori ADFS a tale visualizzazione. La procedura seguente descrive come configurare il monitoraggio delle prestazioni per AD FS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Per configurare il monitoraggio delle prestazioni per ADFS utilizzando Performance Monitor  
  
1.  Nel **avviare** digitare **Performance Monitor**, quindi premere INVIO.  
  
2.  Nell'albero della console, espandere **insiemi agenti di raccolta dati**, a destra\-fare clic su **definito dall'utente**, scegliere **New**, quindi fare clic su **insieme agenti di raccolta dati** .  
  
    Viene visualizzata la Data dell'agente di raccolta impostata procedura guidata Crea nuovo.  
  
3.  Nelle **Crea nuovo insieme di raccolta dati**, per **nome** digitare un nome per il nuovo set di dati dell'agente di raccolta \(, ad esempio "Delle prestazioni di AD FS"\), fare clic su **creare manualmente \( Advanced\)**, quindi fare clic su **successivo**.  
  
4.  Per il tipo di dati da includere, verificare che **creare i log dei dati** sia selezionata e quindi selezionare le caselle di controllo per i tipi di dati seguenti: **Contatore delle prestazioni**, **dati di traccia eventi**, **informazioni sulla configurazione di sistema**.  
  
5.  Per i contatori delle prestazioni, espandere **ADFS** nel **contatori disponibili** elenco e quindi fare clic su **Add**.  
  
    I contatori delle prestazioni di AD FS devono essere visualizzato nei **aggiunti contatori** elenco.  
  
6.  Quando viene chiesto di aggiungere provider di traccia eventi, fare clic su **Add**, selezionare **gestione eventi ADFS** e **traccia di AD FS** dall'elenco dei provider.  
  
7.  Quando viene richiesto di aggiungere le chiavi del Registro di sistema per monitorare, fare clic su **successivo**.  
  
8.  Quando viene chiesto di specificare il percorso in cui salvare i dati sulle prestazioni, è possibile accettare il percorso predefinito \( * *% systemdrive %\\regprest\\Admin\\* * * < data\_collector\_impostare >*, quindi fare clic su **successivo**.  
  
9. Quando viene chiesto di creare il set di dati dell'agente di raccolta, selezionare **salvare e chiudere**, quindi fare clic su **fine**.  
  
    Il nuovo insieme agenti di raccolta dati viene visualizzata nell'albero della console sotto il **definite dall'utente** nodo.  
  
10. Usare i passaggi seguenti per lavorare con i contatori delle prestazioni di AD FS:  
  
    -   Per iniziare il monitoraggio delle prestazioni tramite AD FS\-correlati i contatori, a destra\-fare clic sul set di agenti di raccolta dati che è stato aggiunto \(, ad esempio "Delle prestazioni di AD FS"\), quindi fare clic su **avviare**.  
  
    -   Per creare un report per visualizzare i risultati di monitoraggio delle prestazioni, pulsante destro del mouse\-fare clic sul set di agenti di raccolta dati che è stato aggiunto \(, ad esempio "Delle prestazioni di AD FS"\), quindi fare clic su **Report più recente**.  
  
    -   Per terminare un'acquisizione dei dati sulle prestazioni in modo che sia possibile visualizzare il report più recente, pulsante destro del mouse\-fare clic sul set di agenti di raccolta dati che è stato aggiunto \(, ad esempio "Delle prestazioni di AD FS"\), quindi fare clic su **Arresta**.  
  
    Il report più recente viene aggiunto e numerato automaticamente \(partire 000001\) sotto il **Report\\definita dall'utente *\\< data\_dell'agente di raccolta\_impostato >* nodo nell'albero della console.  
  
## <a name="ad-fs-performance-counters"></a>Contatori delle prestazioni di AD FS  
Nella tabella seguente elenca i contatori delle prestazioni di AD FS e descrive come sono utili per il monitoraggio delle attività che si riferisce a un server federativo o proxy server federativo.  
  
|Contatore|Descrizione|Può essere usato su: 
|-----------|---------------|------------------- 
|Richieste token|Consente di monitorare il numero di richieste di token inviati al server federativo, incluse le richieste di token SSOAuth.|Server federativi 
|Richieste di token\/sec|Consente di monitorare il numero di richieste di token inviati al server federativo, incluse le richieste di token SSOAuth al secondo.|Server federativi  
|Richieste metadati di federazione|Controlla il numero di richieste di metadati federazione in ingresso inviati al server federativo.|Server federativi  
|Richieste di metadati di federazione\/sec|Consente di monitorare il numero di federazione metadati le richieste in ingresso al secondo che vengono inviati al server federativo.|Server federativi  
|Richieste di risoluzione artefatto|Consente di monitorare il numero di federazione metadati le richieste in ingresso al secondo che vengono inviati al server federativo.|Server federativi  
|Le richieste di risoluzione artefatto\/sec|Consente di monitorare il numero di richieste all'endpoint di risoluzione artefatto al secondo che vengono inviati al server federativo.|Server federativi  
|Richieste proxy|Controlla il numero di richieste in ingresso inviati per il proxy server federativo.|Server federativi  
|Le richieste proxy\/sec|Consente di monitorare il numero di richieste in ingresso al secondo che vengono inviati al proxy server federativo.|Server federativi  
|Richieste MEX proxy|Controlla il numero di in ingresso WS\-Metadata Exchange \(MEX\) le richieste inviate al proxy server federativo.|Server federativi 
|Le richieste MEX proxy\/sec|Controlla il numero di richieste in ingresso MEX al secondo che vengono inviati al proxy server federativo.|Server federativi  
  

