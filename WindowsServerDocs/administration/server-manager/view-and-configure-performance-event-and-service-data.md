---
title: Visualizzare e configurare eventi di prestazioni e dei dati
description: Server Manager
ms.prod: windows-server
ms.technology: manage-server-manager
ms.topic: article
ms.assetid: ccd59c35-4dbf-48e7-88a4-c519c00184d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 992fdc6e4f1bba69d540a4ae810bde00db207a46
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851464"
---
# <a name="view-and-configure-performance-event-and-service-data"></a>Visualizzare e configurare dati relativi a prestazioni, eventi e servizi

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene descritto come visualizzare e configurare le voci del registro eventi, contatori delle prestazioni e avvisi di servizio che vengono visualizzati per i server locali e remoti in Server Manager.  

Dati del log eventi, servizio e delle prestazioni viene visualizzati in due posizioni nella console di Server Manager in Windows Server.  

-   Il dashboard, è possibile scegliere il **eventi**, **prestazioni**, e **servizi** righe di anteprime per configurare eventi, prestazioni e i dati di registro del servizio che si desidera visualizzare per i ruoli, l'intero pool di server di gestione Server, i gruppi creati dall'utente di server e il server locale. Facendo clic sulle righe Hypertext vengono aperte le finestre di dialogo **visualizzazione dettagli** che consentono di specificare i dati sui quali si desidera ricevere avvisi nel dashboard. Dopo aver configurato i dati dei registri eventi, di servizio e delle prestazioni che si desidera evidenziare nelle anteprime del dashboard, le voci di log che soddisfano i criteri specificati sono elencate nella parte inferiore delle finestre di dialogo **visualizzazione dettagli** .  

-   I riquadri **Eventi**, **Servizi** e **Prestazioni** fanno parte delle home page di ruoli e gruppi. I comandi nel menu **Attività** di questi riquadri consentono di specificare i dati che dovranno essere raccolti dai server gestiti. Nei riquadri sono disponibili filtri e query per limitare ulteriormente le voci dei registri visualizzate nel riquadro, se lo si desidera.  

In questo argomento sono contenute le seguenti sezioni.  

-   [Cosa sono le anteprime?](#BKMK_thumb)  

-   [Visualizzare e configurare eventi](#BKMK_events)  

-   [Visualizzare e configurare i contatori delle prestazioni](#BKMK_perf)  

-   [Gestire i servizi e configurare gli avvisi del servizio](#BKMK_services)  

-   [Visualizzare e copiare le voci relative a eventi o prestazioni](#BKMK_copy)  

## <a name="what-are-thumbnails"></a><a name=BKMK_thumb></a>Cosa sono le anteprime?  
*Le anteprime* vengono visualizzati nel dashboard di Server Manager per ogni ruolo (anteprima di un ruolo riflette i dati raccolti su tutti i server nel pool di Server Manager che eseguono il ruolo), per ogni gruppo di server, per il **tutti i server** gruppo (tutti i server nel pool di Server Manager) e per il server locale. Dopo la gestione Server Ottiene dati dai server gestiti, le anteprime vengono create automaticamente per i ruoli che sono in esecuzione sui server nel pool di server.  

Se la console di Server Manager è in esecuzione in un computer client come parte del Strumenti di amministrazione remota del server, non è disponibile un'anteprima del **server locale** .  

Nell'anteprima è riprodotta una visualizzazione rapida dello stato e della gestibilità di ruoli, server e gruppi di server. La riga di intestazione dell'anteprima cambia colore (e i numeri evidenziati vengono visualizzati nel margine sinistro) quando gli eventi, i contatori delle prestazioni, Best Practices Analyzer risultati, i servizi o i problemi di gestibilità generali soddisfano i criteri configurati nelle finestre di dialogo **visualizzazione dettagli** aperte facendo clic su anteprime righe. Nella tabella seguente vengono descritti i dati visualizzati nelle anteprime.  

|Riga di anteprima|Descrizione|  
|---------|--------|  
|Gestibilità|La gestibilità di un server include diverse misure: se il server è online o offline, se è accessibile e dati di report a Server Manager, se l'utente connesso al computer locale dispone di diritti utente adeguati per accedere o gestire il server remoto, se il server remoto è in esecuzione tutti i software che sono necessarie a gestirlo in modalità remota , o se il server è configurato in modo da essere eseguita una query e gestiti tramite Server Manager. I dati di gestibilità solo Server Manager possibile raccogliere i dati da un server che esegue Windows Server 2003 sono se il server è online o offline. Per informazioni dettagliate sugli errori di stato di gestibilità e su come risolverli, vedere la [Guida alla risoluzione dei problemi di Server Manager](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).|  
|Eventi|È possibile configurare la riga **Eventi** di un'anteprima per visualizzare avvisi quando vengono registrati gli eventi che corrispondono a livelli di sicurezza, origini, periodi di tempo, server o ID evento specificati dall'utente. Per visualizzare i dettagli degli eventi e modificare gli avvisi che si desidera visualizzare, fare clic sulla riga **eventi** e aprire la finestra di dialogo **visualizzazione dettagli eventi** per il gruppo di ruoli o di server.|  
|Servizi|È possibile configurare la riga **Servizi** per visualizzare avvisi quando i servizi sono disponibili in un gruppo di ruoli o di server che corrisponde a tipi di avvio, stato del servizio, nomi di servizio e server specificati nella finestra di dialogo **visualizzazione dettagli Servizi** .<p>Dopo l'aggiunta di un server al pool di server di gestione Server, possono essere visualizzati avvisi dei servizi relativi al servizio rilevamento Hardware Shell se nessun utente connesso al server gestito. Questo perché il servizio Rilevamento hardware shell viene eseguito solo quando ci sono utenti connessi al server gestito o a una sessione Desktop remoto sul server gestito. Per evitare la visualizzazione degli avvisi relativi al servizio Rilevamento hardware shell per il caso in questione, fare clic su **Servizi** nelle anteprime dei gruppi di server, incluso il gruppo **Tutti i server**. Nella finestra di dialogo **visualizzazione dettagli Servizi** , nell'elenco a discesa **Servizi** deselezionare la casella di controllo **rilevamento hardware shell**, quindi fare clic su **OK**.|  
|Prestazioni|È possibile configurare la riga **prestazioni** per visualizzare avvisi per un ruolo o un gruppo di server quando si verificano avvisi di prestazioni corrispondenti a tipi di risorse, server o periodi di tempo specificati nella finestra di dialogo **visualizzazione Dettagli prestazioni** .<p>Per impostazione predefinita, i contatori delle prestazioni sono disattivati. I server gestiti che eseguono sistemi operativi più recenti di Windows Server 2003 e per i quali non sono stati avviati i contatori delle prestazioni, in genere visualizzano gli errori di stato di gestibilità dei **contatori delle prestazioni online non avviati** nel riquadro **Server** delle pagine ruolo o gruppo. Per attivare i contatori delle prestazioni per i server gestiti, nella pagina **tutti i server** fare clic con il pulsante destro del mouse sulle voci nel riquadro **prestazioni** in cui il valore di **stato contatore** è **disattivato**, quindi scegliere **Avvia contatori prestazioni**. È anche possibile avviare i contatori delle prestazioni facendo clic con il pulsante destro del mouse sulle voci per i server nel riquadro **Server** delle pagine ruolo o gruppo e quindi scegliendo **Avvia contatori prestazioni**.|  
|Risultati BPA|È possibile configurare la riga **dei risultati BPA** per visualizzare avvisi per un ruolo o un gruppo di server quando vengono trovati risultati dell'analisi BPA che corrispondono a livelli di gravità, server o categorie BPA specificati nella finestra di dialogo **visualizzazione Dettagli risultati BPA** .|  

## <a name="view-and-configure-events"></a><a name=BKMK_events></a>Visualizzare e configurare eventi  
In questa sezione, informazioni su come configurare i dati dei registri eventi raccolti dai server nel pool di server di Server Manager e gli eventi che si desidera evidenziare nelle anteprime.  

> [!NOTE]  
> Gli eventi su cui vengono visualizzati avvisi nelle anteprime sono un subset del numero totale di eventi che indicano a Server Manager per raccogliere dai server gestiti. Anche se la modifica di criteri per gli eventi nel **Configura dati evento** nella finestra di dialogo **eventi** riquadri possono modificare il numero di avvisi visualizzati nel dashboard di Server Manager, modifica i criteri di avviso di evento nelle anteprime non ha alcun effetto sui dati dei registri eventi raccolti dai server gestiti.  

#### <a name="to-configure-the-events-collected-from-managed-servers"></a>Per configurare gli eventi raccolti dai server gestiti  

1.  Nella console di Server Manager, aprire una pagina qualsiasi, a eccezione di dashboard. È possibile configurare gli eventi che si desidera raccogliere dai server gestiti nel riquadro **Eventi** nelle pagine del ruolo, del gruppo di server o del server locale.  

2.  Dal menu **Attività**del riquadro **Eventi**scegliere **Configura dati evento**.  

3.  Selezionare i livelli di gravità dell'evento che si desidera raccogliere dai server nel gruppo selezionato. Per impostazione predefinita, sono selezionati i livelli di gravità, **Critico**, **Errore** e **Avviso**.  

4.  Specificare un periodo di tempo entro il quale si verifica l'evento. Il periodo predefinito per gli eventi è di 24 ore.  

5.  Selezionare i file del registro eventi da cui si desidera raccogliere gli eventi. Le impostazioni predefinite sono **Applicazione**, **installazione**e **Sistema**.  

6.  Per salvare le modifiche, fare clic su **OK** per chiudere la finestra di dialogo **Configura dati evento** . I dati evento vengono aggiornati automaticamente al salvataggio delle modifiche.  

#### <a name="to-configure-the-events-highlighted-in-thumbnails"></a>Per configurare gli eventi evidenziati nelle anteprime  

1.  Se Server Manager è già aperto, andare al passaggio successivo. Se Server Manager non è aperto, aprirlo in uno dei modi seguenti.  

    -   Nel desktop di Windows avviare Server Manager facendo clic su **Server Manager** nella barra delle applicazioni di Windows.  

    -   Nella schermata **start** di Windows, seleziona il riquadro Server Manager.  

2.  In un'anteprima nel riquadro **Ruoli e gruppi di server** nella pagina del dashboard fare clic sulla riga **Eventi**.  

3.  Nella finestra di dialogo **visualizzazione dettagli eventi** aggiungere un livello di gravità agli eventi che si desidera visualizzare. Per impostazione predefinita, nelle anteprime vengono visualizzati avvisi evidenziati solo per gli eventi critici. Si noti che il numero di eventi visualizzati nella finestra di dialogo **visualizzazione dettagli** aumenta quando si aggiunge un livello di gravità per il quale si desidera ricevere un avviso.  

4.  Nel campo **Origini eventi** selezionare le origini degli eventi per cui si desidera ricevere avvisi. Il valore predefinito è **All**.  

5.  Se questa anteprima si riferisce a un ruolo installato su più server o a un gruppo di più server, è possibile selezionare i server per i quali si desiderano gli avvisi relativi agli eventi nell'elenco a discesa **Server** .  

6.  Nel campo **periodo di tempo** specificare un periodo di tempo massimo di 1440 minuti, 24 ore o 1 giorno.  

7.  Nel campo**ID evento** digitare i numeri degli ID evento per cui si desidera ricevere avvisi. È possibile digitare un intervallo di ID evento separati da un trattino ( **-** ) ed escludere ID evento dall'intervallo digitando il trattino prima dell'ID evento o di un intervallo di ID evento che si vuole escludere. Ad esempio, il valore **1,3,5-99,-76** significa che vengono generati avvisi per gli ID evento 1 e 3, per tutti gli eventi con ID compreso tra 5 e 99, escluso l'ID evento 76.  

8.  Mentre si modificano i criteri per cui vengono visualizzati avvisi, il numero di avvisi relativi agli eventi visualizzati nel riquadro dei risultati nella parte inferiore della finestra di dialogo potrebbe cambiare. Selezionare le voci nell'elenco e fare clic su **Nascondi avvisi** per impedire che influiscano sul numero di avvisi visualizzati nell'anteprima di origine. Per selezionare più avvisi contemporaneamente, premere e tenere premuto **CTRL** mentre di selezionano gli avvisi. È possibile eseguire questa operazione per gli avvisi corrispondenti ai criteri impostati, ma che non si desidera visualizzare.  

9. Fare clic su **Mostra tutto** per ripristinare la visualizzazione nell'elenco degli avvisi nascosti.  

10. Fare clic su **OK** per salvare le modifiche, chiudere la finestra di dialogo **visualizzazione dettagli** e visualizzare le modifiche degli avvisi relativi agli eventi nell'anteprima di origine.  

## <a name="view-and-configure-performance-log-data"></a><a name=BKMK_perf></a>Visualizzare e configurare i dati dei registri delle prestazioni  
In questa sezione, informazioni su come configurare i dati dei registri delle prestazioni raccolti dai server nel pool di server di Server Manager e il contatore delle prestazioni avvisa l'utente desidera evidenziati nelle anteprime.  

Per impostazione predefinita, i contatori delle prestazioni sono disattivati. I server gestiti che eseguono sistemi operativi più recenti di Windows Server 2003 e per i quali non sono stati avviati i contatori delle prestazioni, in genere visualizzano gli errori di stato di gestibilità dei **contatori delle prestazioni online non avviati** nel riquadro **Server** delle pagine ruolo o gruppo. Per attivare i contatori delle prestazioni per i server gestiti, nella pagina **tutti i server** fare clic con il pulsante destro del mouse sulle voci nel riquadro **prestazioni** in cui il valore di **stato contatore** è **disattivato**, quindi scegliere **Avvia contatori prestazioni**. È anche possibile avviare i contatori delle prestazioni facendo clic con il pulsante destro del mouse sulle voci per i server nel riquadro **Server** delle pagine ruolo o gruppo e quindi scegliendo **Avvia contatori prestazioni**.  

> [!NOTE]  
> Gli avvisi di prestazioni che visualizzando nelle anteprime sono un subset dei dati di contatore delle prestazioni totali che indicano a Server Manager per raccogliere dai server gestiti. Anche se la modifica di criteri di avviso di prestazioni nel **Configura avvisi prestazioni** nella finestra di dialogo **prestazioni** riquadri possono modificare il numero di avvisi visualizzati nel dashboard di Server Manager, modificare i criteri di avviso di prestazioni nelle anteprime non ha alcun effetto sui dati dei log delle prestazioni raccolti dai server gestiti.  
>   
> Di conseguenza, l'età massima dei dati delle prestazioni che è possibile visualizzare nelle anteprime non può essere maggiore del periodo di visualizzazione grafico massimo configurato nella finestra di dialogo **Configura avvisi prestazioni** . Se, ad esempio, il valore **periodo di visualizzazione grafico** in **Configura avvisi prestazioni** è **1 giorno**, il valore massimo per il campo **periodo di tempo** in una finestra di dialogo **visualizzazione Dettagli prestazioni** aperta dal dashboard di Server Manager può essere di **1 giorno**, **24 ore**o **1.440 minuti**.  

#### <a name="to-configure-the-performance-log-data-collected-from-managed-servers"></a>Per configurare dati dei registri delle prestazioni raccolti dai server gestiti  

1.  Nella console di Server Manager, aprire una pagina qualsiasi, a eccezione di dashboard. È possibile configurare i dati delle prestazioni che si desidera raccogliere dai server gestiti nel riquadro **Prestazioni** nelle pagine del ruolo, del gruppo di server o del server locale.  

2.  Per raccogliere dati dei registri delle prestazioni dai server gestiti, è necessario che i contatori delle prestazioni siano attivati. Se i contatori delle prestazioni sono disattivati, fare clic con il pulsante destro del mouse su una voce nell'elenco riquadro **prestazioni** e quindi scegliere **Avvia contatori prestazioni**. La raccolta di dati dei contatori delle prestazioni può richiedere tempo, a seconda del numero di server da cui vengono raccolti i dati e dalla larghezza di banda di rete disponibile. Visualizzare lo stato nella colonna **Stato contatore** .  

3.  Dal menu **Attività** del riquadro **Prestazioni** scegliere **Configura avvisi prestazioni**.  

4.  per i server nel gruppo selezionato, o che eseguono il ruolo selezionato, specificare la percentuale di utilizzo della CPU per gli avvisi dei contatori delle prestazioni raccolti da Server Manager. Il valore predefinito è 85%.  

5.  Specificare la memoria rimanente, espressa in megabyte, che dovrà essere disponibile sui server prima che vengano raccolti gli avvisi dei contatori delle prestazioni. Il valore predefinito è 2 MB.  

6.  Specificare il periodo di tempo visualizzato nei grafici relativi alle risorse **Utilizzo CPU** e **Memoria disponibile** nel riquadro **Prestazioni** della pagina selezionata. Il timeout predefinito è un giorno Fare clic su **Save**.  

    Si noti che il numero di avvisi relativi alle prestazioni nel riquadro **Prestazioni** e il mapping degli avvisi nel tempo visualizzati dal grafico possono cambiare dopo aver fatto clic su **Salva**.  

    > [!NOTE]  
    > per le macchine virtuali con [memoria dinamica](https://technet.microsoft.com/library/ff817651.aspx) attivata, l'aumento della soglia degli avvisi relativi alle prestazioni può generare avvisi falsi positivi.  

7.  Per aggiornare l'elenco degli avvisi relativi alle prestazioni raccolti dai server, scegliere **Aggiorna** dal menu **Attività**.  

#### <a name="to-configure-the-performance-alerts-highlighted-in-thumbnails"></a>Per configurare gli avvisi relativi alle prestazioni evidenziati nelle anteprime  

1.  In un'anteprima nel riquadro **Ruoli e gruppi di server** nella pagina del dashboard fare clic sulla riga **Prestazioni** .  

2.  Nella finestra di dialogo **visualizzazione Dettagli prestazioni** selezionare o deselezionare le caselle di controllo per le soglie di prestazioni delle risorse per cui si desidera ricevere avvisi nel campo **tipo di risorsa** . Si noti che il numero di avvisi relativi alle prestazioni visualizzato nella finestra di dialogo **visualizzazione dettagli** può aumentare quando si aggiunge una soglia per le prestazioni delle risorse per cui si desidera ricevere avvisi.  

3.  Se questa anteprima si riferisce a un ruolo installato su più server o a un gruppo di più server, è possibile selezionare i server per i quali si desiderano gli avvisi relativi alle prestazioni nell'elenco a discesa **Server** .  

4.  Nel campo **periodo di tempo** specificare un periodo di tempo massimo di 1440 minuti, 24 ore o 1 giorno.  

5.  Mentre si modificano i criteri per cui vengono visualizzati avvisi, il numero di avvisi visualizzati nel riquadro dei risultati nella parte inferiore della finestra di dialogo potrebbe cambiare. Fare clic su **Nascondi avvisi** per nascondere tutti gli avvisi precedenti all'ora attuale e impedire che influiscano sul numero di avvisi visualizzati nell'anteprima di origine.  

6.  Fare clic su **Mostra tutto** per ripristinare la visualizzazione nell'elenco degli avvisi nascosti.  

7.  Fare clic su **OK** per salvare le modifiche, chiudere la finestra di dialogo **visualizzazione dettagli** e visualizzare le modifiche degli avvisi relativi alle prestazioni nell'anteprima di origine.  

#### <a name="to-view-the-properties-of-performance-alerts"></a>Per visualizzare le proprietà degli avvisi relativi alle prestazioni  

1.  Effettuare una delle operazioni riportate di seguito.  

    -   In un'anteprima nel riquadro **Ruoli e gruppi di server** nella pagina del dashboard fare clic sulla riga **Prestazioni** .  

    -   Aprire la home page di un ruolo o di un gruppo e individuare il riquadro **Prestazioni** relativo al ruolo o al gruppo.  

2.  Fare doppio clic su un avviso relativo alle prestazioni presente nell'elenco per visualizzarne le proprietà. In alternativa, fare clic con il pulsante destro del mouse su un avviso relativo alle prestazioni e quindi scegliere **Proprietà**.  

3.  Nella finestra di dialogo **Proprietà avviso di prestazioni** selezionare le voci del registro per visualizzare informazioni sui processi associati alla voce nell'area **Processi**.  

4.  Dopo aver visualizzato tali proprietà, chiudere la finestra di dialogo.  

### <a name="analyze-performance-data-and-solve-problems"></a>Analizzare i dati relativi alle prestazioni e risolvere i problemi  
Per ulteriori informazioni sull'analisi dei dati dei contatori delle prestazioni visualizzati in Server Manager e sulla risoluzione dei problemi di prestazioni sui server gestiti, vedere le risorse seguenti.  

-   [Analisi dei dati sulle prestazioni](https://go.microsoft.com/fwlink/?LinkId=239829)  

-   [Risoluzione dei problemi di prestazioni](https://go.microsoft.com/fwlink/?LinkId=239831)  

Per ulteriori informazioni sugli strumenti avanzati di monitoraggio e analisi delle prestazioni disponibili per Windows Server 2012 e versioni successive di Windows Server, tra cui Server Performance Advisor 3,0, vedere [prestazioni](https://msdn.microsoft.com/windows/hardware/gg463374.aspx) su MSDN.  

## <a name="manage-services-and-configure-service-alerts"></a><a name=BKMK_services></a>Gestire i servizi e configurare gli avvisi del servizio  
In questa sezione, informazioni su come avviare, arrestare, riavviare, sospendere o riprendere i servizi vengono visualizzati di **servizi** riquadro nelle pagine di gruppo per il ruolo e server in Server Manager. È anche possibile configurare i servizi su cui vengono visualizzati avvisi nelle anteprime nel dashboard di Server Manager.  

> [!NOTE]  
> È possibile modificare il tipo di avvio per i servizi, le dipendenze del servizio, le opzioni di ripristino o altre proprietà del servizio nel riquadro Servizi in Server Manager. Per modificare le proprietà dei servizi diverse dallo stato del servizio, aprire lo snap-in **Servizi**. Un collegamento per aprire la **servizi** snap-in è disponibile il **strumenti** menu in Server Manager.  

#### <a name="to-start-stop-restart-pause-or-resume-a-service"></a>Per avviare, arrestare, riavviare, sospendere e riprendere un servizio  

1.  Nella console di Server Manager, aprire una pagina qualsiasi, a eccezione di dashboard (in altre parole, qualsiasi ruolo o gruppo la home page).  

2.  Nel riquadro **Servizi**del ruolo o del gruppo fare clic con il pulsante destro del mouse su un servizio.  

3.  Nel menu di scelta rapida scegliere l'azione che si desidera eseguire nel servizio. Se il servizio viene arrestato, la sola azione che è possibile eseguire è l'avvio del servizio. In modo analogo, se il servizio viene sospeso, la sola azione che è possibile eseguire è la ripresa del servizio.  

4.  Quando un servizio viene avviato, arrestato, sospeso o ripreso, il valore della colonna **Stato** per il servizio viene modificato nel riquadro **Servizi**.  

#### <a name="to-configure-service-alerts-highlighted-in-thumbnails"></a>Per configurare avvisi relativi ai servizi evidenziati nelle anteprime  

1.  In un'anteprima nel riquadro **Ruoli e gruppi di server** nella pagina del dashboard fare clic sulla riga **Servizi**.  

2.  Nella finestra di dialogo **visualizzazione dettagli Servizi** selezionare i tipi di avvio per i servizi per i quali si desidera ricevere avvisi. Per impostazione predefinita, sono selezionati **automaticamente (avvio ritardato)** e **automatico** .  

3.  Selezionare gli Stati del servizio per cui si desidera ricevere avvisi. Per impostazione predefinita, è selezionato **Tutti** .  

4.  Selezionare i servizi per cui si desidera ricevere avvisi. Per impostazione predefinita, è selezionato **Tutti** .  

5.  Selezionare i server associati al ruolo o al gruppo per cui si desidera ricevere avvisi relativi ai servizi. Per impostazione predefinita, è selezionato **Tutti** .  

6.  Mentre si modificano i criteri per cui vengono visualizzati avvisi, il numero di avvisi visualizzati nel riquadro dei risultati nella parte inferiore della finestra di dialogo potrebbe cambiare. Fare clic su **Nascondi avvisi** per nascondere tutti gli avvisi precedenti all'ora attuale e impedire che influiscano sul numero di avvisi visualizzati nell'anteprima di origine.  

7.  Fare clic su **Mostra tutto** per ripristinare la visualizzazione nell'elenco degli avvisi nascosti.  

8.  Fare clic su **OK** per salvare le modifiche, chiudere la finestra di dialogo **visualizzazione dettagli** e visualizzare le modifiche agli avvisi del servizio nell'anteprima di origine.  

## <a name="view-and-copy-event-service-or-performance-entries"></a><a name=BKMK_copy></a>Visualizzare e copiare le voci relative a eventi, servizi o prestazioni  
È possibile copiare le proprietà delle voci relative a eventi, servizi o prestazioni nelle finestre di dialogo **visualizzazione dettagli** e nei riquadri **eventi** e **prestazioni** per un ruolo o un gruppo. Fare clic con il pulsante destro del mouse su una voce relativa a un evento o prestazioni, quindi scegliere **copia**.  

È inoltre possibile visualizzare in anteprima le proprietà degli eventi nella metà inferiore del riquadro **Eventi** , selezionando un evento dall'elenco. Per copiare le proprietà visualizzate nell'anteprima, fare clic con il pulsante destro del mouse sul riquadro di anteprima, quindi scegliere **copia**.  

## <a name="see-also"></a>Vedi anche  
[Server Manager](server-manager.md)  
[Filtrare e ordinare i dati ed eseguire query su di essi nei riquadri di Server Manager](filter-sort-and-query-data-in-server-manager-tiles.md)  
