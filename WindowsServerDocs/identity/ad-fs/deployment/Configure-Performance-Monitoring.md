---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: Configurare il monitoraggio delle prestazioni
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6a7602cddcaee274d42213cd9365f6d1722dab79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="configure-performance-monitoring"></a>Configurare il monitoraggio delle prestazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="bkmk_ConfigurePerfMon"></a>  
ADFS include un proprio contatori delle prestazioni dedicati che consentono di monitorare le prestazioni di server federativi e i computer proxy server federativo. Per utilizzare Performance Monitor per monitorare le prestazioni dei server ADFS, è utile creare un nuovo insieme agenti di raccolta dati e aggiungere i contatori ADFS a tale visualizzazione. La procedura seguente viene descritto come configurare il monitoraggio delle prestazioni per ADFS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Per configurare il monitoraggio delle prestazioni per ADFS con Performance Monitor  
  
1.  Nel **Start** digitare **Performance Monitor**, quindi premere INVIO.  
  
2.  Nell'albero della console, espandere **insiemi agenti di raccolta dati**, fare clic con **definiti dall'utente**, scegliere **New**, quindi fare clic su **insieme agenti di raccolta dati**.  
  
    Viene visualizzato il dati agente di raccolta dati impostare procedura guidata Crea nuovo.  
  
3.  In **creare nuovo insieme di raccolta dati**, per **nome** digitare un nome per il nuovo insieme agenti di raccolta dati \ (ad esempio "Prestazioni AD FS" \), fare clic su **creare manualmente \(Advanced\)**, quindi fare clic su **Avanti**.  
  
4.  Per il tipo di dati da includere, verificare che **creare registri di dati** sia selezionata e quindi selezionare le caselle di controllo per i seguenti tipi di dati: **contatore delle prestazioni**, **dati di traccia eventi**, **informazioni sulla configurazione di sistema**.  
  
5.  Per i contatori delle prestazioni, espandere **ADFS** nel **contatori disponibili** elenco e quindi fare clic su **Aggiungi**.  
  
    I contatori delle prestazioni di AD FS dovrebbe essere visualizzato nel **aggiunti contatori** elenco.  
  
6.  Quando viene chiesto di aggiungere provider di traccia eventi, fare clic su **Aggiungi**selezionare **AD FS Eventing** e **traccia di AD FS** dall'elenco dei provider.  
  
7.  Quando viene richiesto di aggiungere le chiavi del Registro di sistema per il monitoraggio, fare clic su **Avanti**.  
  
8.  Quando viene chiesto di specificare il percorso per salvare i dati sulle prestazioni, è possibile accettare il percorso predefinito \ (**%systemdrive%\\PerfLogs\\Admin\\***< data\_collector\_set >*, quindi fare clic su **Avanti**.  
  
9. Quando viene chiesto di creare l'insieme agenti di raccolta dati, selezionare **salvare e chiudere**, quindi fare clic su **fine**.  
  
    Il nuovo insieme agenti di raccolta dati viene visualizzata nell'albero della console sotto il **definiti dall'utente** nodo.  
  
10. Usare la procedura seguente per lavorare con i contatori delle prestazioni di AD FS:  
  
    -   Per iniziare il monitoraggio delle prestazioni utilizzando i contatori relativi AD FS\, fare clic con l'agente di raccolta dati impostare che è stato aggiunto \ (ad esempio "Prestazioni AD FS" \), quindi fare clic su **Start**.  
  
    -   Per creare un report per visualizzare i risultati di monitoraggio delle prestazioni, fare clic con l'agente di raccolta dati impostare che è stato aggiunto \ (ad esempio "Prestazioni AD FS" \), quindi fare clic su **Report più recente**.  
  
    -   Per terminare un'acquisizione dei dati sulle prestazioni in modo che è possibile visualizzare il report più recente, fare clic con l'agente di raccolta dati impostare che è stato aggiunto \ (ad esempio "Prestazioni AD FS" \), quindi fare clic su **arrestare**.  
  
    Il report più recente viene aggiunto e numerato automaticamente \ (a partire da 000001\) sotto il **Report\\User definiti***\ \ < data\_collector\_set >* nodo nell'albero della console.  
  
## <a name="ad-fs-performance-counters"></a>Contatori delle prestazioni di AD FS  
Nella tabella seguente elenca i contatori delle prestazioni di AD FS e descrive come sono utili per monitorare l'attività che fa riferimento a un server federativo o proxy server federativo.  
  
|Contatore|Descrizione|Può essere utilizzato in: 
|-----------|---------------|------------------- 
|Richieste di token|Controlla il numero di richieste di token inviati al server federativo, tra cui le richieste di token SSOAuth.|Server federativi 
|Token Requests\ al secondo|Controlla il numero di richieste di token inviati al server federativo, tra cui le richieste di token SSOAuth al secondo.|Server federativi  
|Richieste di metadati federativi|Controlla il numero di richieste in ingresso dei metadati di federazione inviati al server federativo.|Server federativi  
|Federazione metadati Requests\ al secondo|Controlla il numero di richieste in ingresso federazione metadati al secondo che vengono inviati al server federativo.|Server federativi  
|Richieste di risoluzione artefatto|Controlla il numero di richieste in ingresso federazione metadati al secondo che vengono inviati al server federativo.|Server federativi  
|Requests\ risoluzione artefatto al secondo|Controlla il numero di richieste inviate al server federativo per l'endpoint di risoluzione artefatto al secondo.|Server federativi  
|Richieste proxy|Controlla il numero di richieste in ingresso inviati per il proxy server federativo.|I proxy Server federativi  
|Proxy Requests\ al secondo|Controlla il numero di richieste in ingresso al secondo che vengono inviati per il proxy server federativo.|I proxy Server federativi  
|Richieste MEX proxy|Controlla il numero di richieste in ingresso di WS-Metadata Exchange \(MEX\) inviate al proxy server federativo.|I proxy Server federativi 
|Proxy MEX Requests\ al secondo|Controlla il numero di richieste in ingresso MEX al secondo che vengono inviati per il proxy server federativo.|I proxy Server federativi  
  

