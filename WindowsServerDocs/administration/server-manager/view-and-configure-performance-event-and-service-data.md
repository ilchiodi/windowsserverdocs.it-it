---
title: Visualizzare e configurare eventi di prestazioni e dei dati
description: Server Manager
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ccd59c35-4dbf-48e7-88a4-c519c00184d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 727b81d1ba2cda32a7568e4e2f23065b1589e745
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861902"
---
# <a name="view-and-configure-performance-event-and-service-data"></a>Visualizzare e configurare dati relativi a prestazioni, eventi e servizi

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene descritto come visualizzare e configurare le voci del registro eventi, contatori delle prestazioni e avvisi di servizio che vengono visualizzati per i server locali e remoti in Server Manager.  

Dati del log eventi, servizio e delle prestazioni viene visualizzati in due posizioni nella console di Server Manager in Windows Server.  

-   Il dashboard, è possibile scegliere il **eventi**, **prestazioni**, e **servizi** righe di anteprime per configurare eventi, prestazioni e i dati di registro del servizio che si desidera visualizzare per i ruoli, l'intero pool di server di gestione Server, i gruppi creati dall'utente di server e il server locale. Facendo clic sull'ipertesto righe aperte **visualizzazione dettagli** finestre di dialogo che consentono di specificano i dati su cui si desidera ricevere avvisi nel dashboard. Dopo aver configurato l'evento, servizio e i dati di log delle prestazioni che si desidera evidenziare nelle anteprime del dashboard, nella parte inferiore della sono elencate le voci di log che corrispondono ai criteri è stato specificato il **visualizzazione dettagli** finestre di dialogo.  

-   I riquadri **Eventi**, **Servizi** e **Prestazioni** fanno parte delle home page di ruoli e gruppi. I comandi nel menu **Attività** di questi riquadri consentono di specificare i dati che dovranno essere raccolti dai server gestiti. Nei riquadri sono disponibili filtri e query per limitare ulteriormente le voci dei registri visualizzate nel riquadro, se lo si desidera.  

In questo argomento sono incluse le sezioni seguenti.  

-   [Che cosa sono le anteprime](#BKMK_thumb)  

-   [Visualizzare e configurare gli eventi](#BKMK_events)  

-   [Visualizzare e configurare i contatori delle prestazioni](#BKMK_perf)  

-   [Gestire i servizi e configurare gli avvisi del servizio](#BKMK_services)  

-   [Visualizzare e copiare le voci evento o alle prestazioni](#BKMK_copy)  

## <a name="BKMK_thumb"></a>Che cosa sono le anteprime  
*Le anteprime* vengono visualizzati nel dashboard di Server Manager per ogni ruolo (anteprima di un ruolo riflette i dati raccolti su tutti i server nel pool di Server Manager che eseguono il ruolo), per ogni gruppo di server, per il **tutti i server** gruppo (tutti i server nel pool di Server Manager) e per il server locale. Dopo la gestione Server Ottiene dati dai server gestiti, le anteprime vengono create automaticamente per i ruoli che sono in esecuzione sui server nel pool di server.  

Se la console di Server Manager è in esecuzione in un computer client come parte di strumenti di amministrazione remota del Server, è presente alcun **Server locale** anteprima.  

Nell'anteprima è riprodotta una visualizzazione rapida dello stato e della gestibilità di ruoli, server e gruppi di server. La riga di intestazione dell'anteprima cambia colore (e i numeri evidenziati vengono visualizzati nel margine sinistro) quando gli eventi, contatori delle prestazioni, risultati Best Practices Analyzer, servizi o problemi di gestibilità generali soddisfano i criteri configurati nel **visualizzazione dettagli** finestre di dialogo aperte facendo clic sulle righe dell'anteprima. Nella tabella seguente vengono descritti i dati visualizzati nelle anteprime.  

|Riga di anteprima|Descrizione|  
|---------|--------|  
|Gestibilità|La gestibilità di un server include diverse misure: se il server è online o offline, se è accessibile e dati di report a Server Manager, se l'utente connesso al computer locale dispone di diritti utente adeguati per accedere o gestire il server remoto, se il server remoto è in esecuzione tutti i software che sono necessarie a gestirlo in modalità remota , o se il server è configurato in modo da essere eseguita una query e gestiti tramite Server Manager. I dati di gestibilità solo Server Manager possibile raccogliere i dati da un server che esegue Windows Server 2003 sono se il server è online o offline. Per informazioni dettagliate sugli errori di stato di gestibilità e su come risolverli, vedere la [Guida alla risoluzione dei problemi di Server Manager](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).|  
|Eventi|È possibile configurare la riga **Eventi** di un'anteprima per visualizzare avvisi quando vengono registrati gli eventi che corrispondono a livelli di sicurezza, origini, periodi di tempo, server o ID evento specificati dall'utente. Visualizzare i dettagli sugli eventi e modificare gli avvisi che si desidera visualizzare, fare clic il **eventi** di riga e aprire il **visualizzazione Dettagli eventi** finestra di dialogo per il gruppo di ruoli o di server.|  
|Servizi|È possibile configurare il **Services** riga per visualizzare avvisi quando si trovano in un gruppo di ruoli o di server che corrispondono a tipi di avvio, lo stato del servizio, i nomi dei servizi e server specificati in servizi di **visualizzazione dettagli servizi**  nella finestra di dialogo.<br /><br />Dopo l'aggiunta di un server al pool di server di gestione Server, possono essere visualizzati avvisi dei servizi relativi al servizio rilevamento Hardware Shell se nessun utente connesso al server gestito. Questo perché il servizio Rilevamento hardware shell viene eseguito solo quando ci sono utenti connessi al server gestito o a una sessione Desktop remoto sul server gestito. Per evitare la visualizzazione degli avvisi relativi al servizio Rilevamento hardware shell per il caso in questione, fare clic su **Servizi** nelle anteprime dei gruppi di server, incluso il gruppo **Tutti i server**. Nel **visualizzazione dettagli servizi** finestra di dialogo il **servizi** elenco a discesa, deselezionare il casella di controllo **rilevamento Hardware Shell**, quindi fare clic su **OK**.|  
|Prestazioni|È possibile configurare il **prestazioni** riga per visualizzare gli avvisi per un ruolo o gruppo di server quando si verificano avvisi di prestazioni che corrispondono a tipi di risorse, server o periodi di tempo specificato nella **dettaglio prestazioni View** finestra di dialogo.<br /><br />Per impostazione predefinita, i contatori delle prestazioni sono disattivati. Server gestiti che eseguono sistemi operativi più recenti rispetto a Windows Server 2003 e le prestazioni contatori non sono stati avviati, in genere Mostra gestibilità errori dello stato **online - contatori delle prestazioni non avviati** nel **server** affiancato di pagine ruolo o gruppo. Per attivare i contatori delle prestazioni dei server gestiti, nella **tutti i server** pagina, fare doppio clic su voci nel **prestazioni** riquadro che mostrano un **stato contatore** valore  **Disattivare**, quindi fare clic su **avviare i contatori delle prestazioni**. È anche possibile avviare i contatori delle prestazioni facendo le voci per i server di **i server** riquadro del ruolo o le pagine di gruppo e quindi facendo clic su **avviare i contatori delle prestazioni**.|  
|Risultati BPA|È possibile configurare il **i risultati BPA** riga per visualizzare gli avvisi per un gruppo di ruoli o di server quando i risultati dell'analisi BPA sono disponibili che corrispondono ai livelli di gravità, server o categorie BPA specificati nella **visualizzazioneDettaglirisultatiBPA** finestra di dialogo.|  

## <a name="BKMK_events"></a>Visualizzare e configurare gli eventi  
In questa sezione, informazioni su come configurare i dati dei registri eventi raccolti dai server nel pool di server di Server Manager e gli eventi che si desidera evidenziare nelle anteprime.  

> [!NOTE]  
> Gli eventi su cui vengono visualizzati avvisi nelle anteprime sono un subset del numero totale di eventi che indicano a Server Manager per raccogliere dai server gestiti. Anche se la modifica di criteri per gli eventi nel **Configura dati evento** nella finestra di dialogo **eventi** riquadri possono modificare il numero di avvisi visualizzati nel dashboard di Server Manager, modifica i criteri di avviso di evento nelle anteprime non ha alcun effetto sui dati dei registri eventi raccolti dai server gestiti.  

#### <a name="to-configure-the-events-collected-from-managed-servers"></a>Per configurare gli eventi raccolti dai server gestiti  

1.  Nella console di Server Manager, aprire una pagina qualsiasi, a eccezione di dashboard. È possibile configurare gli eventi che si desidera raccogliere dai server gestiti nel riquadro **Eventi** nelle pagine del ruolo, del gruppo di server o del server locale.  

2.  Dal menu **Attività**del riquadro **Eventi**scegliere **Configura dati evento**.  

3.  Selezionare i livelli di gravità evento che si desidera raccogliere dai server nel gruppo selezionato. Per impostazione predefinita, sono selezionati i livelli di gravità, **Critico**, **Errore** e **Avviso**.  

4.  Specificare un periodo di tempo entro il quale si verifica l'evento. Il periodo predefinito per gli eventi è di 24 ore.  

5.  Selezionare i file di registro eventi da cui si desidera che gli eventi da raccogliere. Le impostazioni predefinite sono **Applicazione**, **installazione**e **Sistema**.  

6.  Per salvare le modifiche, fare clic su **OK** per chiudere la finestra di dialogo **Configura dati evento** . I dati evento vengono aggiornati automaticamente al salvataggio delle modifiche.  

#### <a name="to-configure-the-events-highlighted-in-thumbnails"></a>Per configurare gli eventi evidenziati nelle anteprime  

1.  Se Server Manager è già aperto, andare al passaggio successivo. Se Server Manager non è aperto, aprirlo in uno dei modi seguenti.  

    -   Nel desktop di Windows avviare Server Manager facendo clic su **Server Manager** nella barra delle applicazioni di Windows.  

    -   Nella schermata **start** di Windows, seleziona il riquadro Server Manager.  

2.  In un'anteprima nel riquadro **Ruoli e gruppi di server** nella pagina del dashboard fare clic sulla riga **Eventi**.  

3.  Nel **visualizzazione Dettagli eventi** finestra di dialogo, aggiungere un livello di gravità agli eventi che si desidera visualizzare. Per impostazione predefinita, nelle anteprime vengono visualizzati avvisi evidenziati solo per gli eventi critici. Il numero di eventi riprodotti nella **visualizzazione dettagli** nella finestra di dialogo aumenta quando si aggiunge un livello di gravità intorno al quale si desidera ricevere avvisi.  

4.  Nel campo **Origini eventi** selezionare le origini degli eventi per cui si desidera ricevere avvisi. Il valore predefinito è **All**.  

5.  Se questa anteprima si riferisce a un ruolo installato su più server o un gruppo di più server, è possibile selezionare i server per il quale si desidera ricevere avvisi relativi eventi nel **server** elenco a discesa.  

6.  Nel **periodo di tempo** campo, specificare un periodo di tempo massima di 1440 minuti, 24 ore o 1 giorno.  

7.  Nel campo**ID evento** digitare i numeri degli ID evento per cui si desidera ricevere avvisi. È possibile digitare un intervallo di ID evento separati da un trattino (**-**) ed escludere ID evento dall'intervallo digitando il trattino prima dell'ID evento o di un intervallo di ID evento che si vuole escludere. Ad esempio, il valore **1,3,5-99,-76** significa che vengono generati avvisi per gli ID evento 1 e 3, per tutti gli eventi con ID compreso tra 5 e 99, escluso l'ID evento 76.  

8.  Mentre si modificano i criteri per cui vengono visualizzati avvisi, il numero di avvisi relativi agli eventi visualizzati nel riquadro dei risultati nella parte inferiore della finestra di dialogo potrebbe cambiare. Selezionare le voci nell'elenco e fare clic su **Nascondi avvisi** per impedire che influiscano sul numero di avviso viene visualizzato in anteprima di origine. Per selezionare più avvisi contemporaneamente, premere e tenere premuto **CTRL** mentre di selezionano gli avvisi. È possibile eseguire questa operazione per gli avvisi corrispondenti ai criteri impostati, ma che non si desidera visualizzare.  

9. Fare clic su **Mostra tutto** per ripristinare la visualizzazione nell'elenco degli avvisi nascosti.  

10. Fare clic su **OK** per salvare le modifiche, chiudere il **visualizzazione dettagli** nella finestra di dialogo e visualizzare l'avviso per evento modifiche nell'anteprima di origine.  

## <a name="BKMK_perf"></a>Visualizzare e configurare dati di log delle prestazioni  
In questa sezione, informazioni su come configurare i dati dei registri delle prestazioni raccolti dai server nel pool di server di Server Manager e il contatore delle prestazioni avvisa l'utente desidera evidenziati nelle anteprime.  

Per impostazione predefinita, i contatori delle prestazioni sono disattivati. Server gestiti che eseguono sistemi operativi più recenti rispetto a Windows Server 2003 e le prestazioni contatori non sono stati avviati, in genere Mostra gestibilità errori dello stato **online - contatori delle prestazioni non avviati** nel **server** affiancato di pagine ruolo o gruppo. Per attivare i contatori delle prestazioni dei server gestiti, nella **tutti i server** pagina, fare doppio clic su voci nel **prestazioni** riquadro che mostrano un **stato contatore** valore  **Disattivare**, quindi fare clic su **avviare i contatori delle prestazioni**. È anche possibile avviare i contatori delle prestazioni facendo le voci per i server di **i server** riquadro del ruolo o le pagine di gruppo e quindi facendo clic su **avviare i contatori delle prestazioni**.  

> [!NOTE]  
> Gli avvisi di prestazioni che visualizzando nelle anteprime sono un subset dei dati di contatore delle prestazioni totali che indicano a Server Manager per raccogliere dai server gestiti. Anche se la modifica di criteri di avviso di prestazioni nel **Configura avvisi prestazioni** nella finestra di dialogo **prestazioni** riquadri possono modificare il numero di avvisi visualizzati nel dashboard di Server Manager, modificare i criteri di avviso di prestazioni nelle anteprime non ha alcun effetto sui dati dei log delle prestazioni raccolti dai server gestiti.  
>   
> Di conseguenza, l'età massima dei dati delle prestazioni che è possibile visualizzare nelle anteprime non può essere maggiore del periodo di visualizzazione grafico massimo configurato nella finestra di dialogo **Configura avvisi prestazioni** . Ad esempio, se il **periodo di visualizzazione grafico** valore nelle **Configura avvisi prestazioni** è **1 giorno**, il valore massimo per il **il periodo di tempo**campo in un **dettaglio prestazioni View** finestra di dialogo che è aperta dal dashboard di Server Manager può essere **1 giorno**, **24 ore**, o **1.440 minuti**.  

#### <a name="to-configure-the-performance-log-data-collected-from-managed-servers"></a>Per configurare dati dei registri delle prestazioni raccolti dai server gestiti  

1.  Nella console di Server Manager, aprire una pagina qualsiasi, a eccezione di dashboard. È possibile configurare i dati delle prestazioni che si desidera raccogliere dai server gestiti nel riquadro **Prestazioni** nelle pagine del ruolo, del gruppo di server o del server locale.  

2.  Per raccogliere dati dei registri delle prestazioni dai server gestiti, è necessario che i contatori delle prestazioni siano attivati. Se i contatori delle prestazioni sono disattivati, fare doppio clic su una voce nella **Performance** riquadro elenco e quindi fare clic su **avviare i contatori delle prestazioni**. La raccolta di dati dei contatori delle prestazioni può richiedere tempo, a seconda del numero di server da cui vengono raccolti i dati e dalla larghezza di banda di rete disponibile. Visualizzare lo stato nella colonna **Stato contatore** .  

3.  Dal menu **Attività** del riquadro **Prestazioni** scegliere **Configura avvisi prestazioni**.  

4.  per i server nel gruppo selezionato, o che eseguono il ruolo selezionato, specificare la percentuale di utilizzo della CPU si desidera che gli avvisi dei contatori delle prestazioni raccolti da Server Manager. Il valore predefinito è 85%.  

5.  Specificare la memoria rimanente, espressa in megabyte, che dovrà essere disponibile sui server prima che vengano raccolti gli avvisi dei contatori delle prestazioni. Il valore predefinito è 2 MB.  

6.  Specificare il periodo di tempo visualizzato nei grafici relativi alle risorse **Utilizzo CPU** e **Memoria disponibile** nel riquadro **Prestazioni** della pagina selezionata. Il timeout predefinito è un giorno Fare clic su **Salva**.  

    Si noti che il numero di avvisi relativi alle prestazioni nel riquadro **Prestazioni** e il mapping degli avvisi nel tempo visualizzati dal grafico possono cambiare dopo aver fatto clic su **Salva**.  

    > [!NOTE]  
    > per le macchine virtuali che hanno [memoria dinamica](https://technet.microsoft.com/library/ff817651.aspx) attivata, l'aumento della soglia degli avvisi di prestazioni può comportare falsi allarmi.  

7.  Per aggiornare l'elenco degli avvisi relativi alle prestazioni raccolti dai server, scegliere **Aggiorna** dal menu **Attività**.  

#### <a name="to-configure-the-performance-alerts-highlighted-in-thumbnails"></a>Per configurare gli avvisi relativi alle prestazioni evidenziati nelle anteprime  

1.  In un'anteprima nel riquadro **Ruoli e gruppi di server** nella pagina del dashboard fare clic sulla riga **Prestazioni** .  

2.  Nel **dettaglio prestazioni vista** finestra di dialogo, selezionare o deselezionare le caselle di controllo per le soglie delle prestazioni di risorse su cui si desidera ricevere avvisi nel **tipo di risorsa** campo. Il numero di avvisi relativi alle prestazioni riprodotti nella **visualizzazione dettagli** nella finestra di dialogo può aumentare quando si aggiunge una soglia di prestazioni di risorse su cui si vuole essere avvisati.  

3.  Se questa anteprima si riferisce a un ruolo installato su più server o un gruppo di più server, è possibile selezionare il server per il quale si desidera avvisi relativi alle prestazioni nel **server** elenco a discesa.  

4.  Nel **periodo di tempo** campo, specificare un periodo di tempo massima di 1440 minuti, 24 ore o 1 giorno.  

5.  Mentre si modificano i criteri per cui vengono visualizzati avvisi, il numero di avvisi visualizzati nel riquadro dei risultati nella parte inferiore della finestra di dialogo potrebbe cambiare. Fare clic su **Nascondi avvisi** per nascondere tutti gli avvisi precedenti all'ora attuale e impedire che influiscano sul numero di avvisi visualizzati nell'anteprima di origine.  

6.  Fare clic su **Mostra tutto** per ripristinare la visualizzazione nell'elenco degli avvisi nascosti.  

7.  Fare clic su **OK** per salvare le modifiche, chiudere il **visualizzazione dettagli** nella finestra di dialogo e visualizzare l'avviso di prestazione modifiche nell'anteprima di origine.  

#### <a name="to-view-the-properties-of-performance-alerts"></a>Per visualizzare le proprietà degli avvisi relativi alle prestazioni  

1.  Effettuare una delle operazioni seguenti.  

    -   In un'anteprima nel riquadro **Ruoli e gruppi di server** nella pagina del dashboard fare clic sulla riga **Prestazioni** .  

    -   Aprire la home page di un ruolo o di un gruppo e individuare il riquadro **Prestazioni** relativo al ruolo o al gruppo.  

2.  Fare doppio clic su un avviso relativo alle prestazioni presente nell'elenco per visualizzarne le proprietà. In alternativa, fare clic con il pulsante destro del mouse su un avviso relativo alle prestazioni e quindi scegliere **Proprietà**.  

3.  Nella finestra di dialogo **Proprietà avviso di prestazioni** selezionare le voci del registro per visualizzare informazioni sui processi associati alla voce nell'area **Processi**.  

4.  Dopo aver visualizzato tali proprietà, chiudere la finestra di dialogo.  

### <a name="analyze-performance-data-and-solve-problems"></a>Analizzare i dati relativi alle prestazioni e risolvere i problemi  
per altre informazioni sull'analisi dei dati dei contatori delle prestazioni visualizzati in Server Manager e risoluzione dei problemi di prestazioni sui server gestiti, vedere le risorse seguenti.  

-   [L'analisi dei dati sulle prestazioni](https://go.microsoft.com/fwlink/?LinkId=239829)  

-   [Risoluzione dei problemi di prestazioni](https://go.microsoft.com/fwlink/?LinkId=239831)  

per altre informazioni sulle prestazioni avanzate gli strumenti di analisi e monitoraggio disponibili per Windows Server 2012 e versioni successive di Windows Server, incluso Server Performance Advisor 3.0, vedere [prestazioni](https://msdn.microsoft.com/windows/hardware/gg463374.aspx) su MSDN.  

## <a name="BKMK_services"></a>Gestire i servizi e configurare gli avvisi del servizio  
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

2.  Nel **visualizzazione dettagli servizi** finestra di dialogo, seleziona i tipi di avvio per i servizi su cui si vuole essere avvisati. Per impostazione predefinita **automatico (avvio ritardato)** e **automatica** siano selezionate.  

3.  Selezionare gli stati del servizio su cui si vuole essere avvisati. Per impostazione predefinita, è selezionato **Tutti** .  

4.  Selezionare i servizi su cui si vuole essere avvisati. Per impostazione predefinita, è selezionato **Tutti** .  

5.  Selezionare i server associati al ruolo o gruppo per il quale si desidera ricevere avvisi relativi ai servizi. Per impostazione predefinita, è selezionato **Tutti** .  

6.  Mentre si modificano i criteri per cui vengono visualizzati avvisi, il numero di avvisi visualizzati nel riquadro dei risultati nella parte inferiore della finestra di dialogo potrebbe cambiare. Fare clic su **Nascondi avvisi** per nascondere tutti gli avvisi precedenti all'ora attuale e impedire che influiscano sul numero di avvisi visualizzati nell'anteprima di origine.  

7.  Fare clic su **Mostra tutto** per ripristinare la visualizzazione nell'elenco degli avvisi nascosti.  

8.  Fare clic su **OK** per salvare le modifiche, chiudere il **visualizzazione dettagli** nella finestra di dialogo e visualizzare l'avviso di assistenza modifiche nell'anteprima di origine.  

## <a name="BKMK_copy"></a>Visualizzare e copiare le voci evento, servizio o prestazioni  
È possibile copiare le proprietà di voce di evento, servizio o prestazioni nel **visualizzazione dettagli** finestre di dialogo e i **eventi** e **prestazioni** riquadri per un ruolo o gruppo. Fare doppio clic su una voce di evento o alle prestazioni e quindi fare clic su **copia**.  

È inoltre possibile visualizzare in anteprima le proprietà degli eventi nella metà inferiore del riquadro **Eventi** , selezionando un evento dall'elenco. Per copiare le proprietà visualizzate nell'anteprima, fare clic con il pulsante destro del riquadro di anteprima e quindi fare clic su **copia**.  

## <a name="see-also"></a>Vedere anche  
[Server Manager](server-manager.md)  
[Filtrare, ordinare ed eseguire query sui dati nei riquadri di Server Manager](filter-sort-and-query-data-in-server-manager-tiles.md)  
