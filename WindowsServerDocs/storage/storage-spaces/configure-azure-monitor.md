---
title: Informazioni e configurazione di monitoraggio di Azure
description: Informazioni dettagliate sulla configurazione di monitoraggio di Azure e su come configurare gli avvisi di posta elettronica e SMS per il cluster con spazi di archiviazione diretta in Windows Server 2016 e 2019.
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/10/2020
ms.openlocfilehash: fa4d8793c07cabd39ee6cc0d54b5cddc84eac202
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859044"
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>Usare monitoraggio di Azure per inviare messaggi di posta elettronica per gli errori di Servizio integrità

>Si applica a: Windows Server 2019, Windows Server 2016

Monitoraggio di Azure massimizza la disponibilità e le prestazioni delle applicazioni grazie a una soluzione completa per la raccolta, l'analisi e la gestione dei dati di telemetria dagli ambienti cloud e locali. Consente di comprendere in che modo le applicazioni eseguono e identifica in modo proattivo i problemi che interessano le risorse e le risorse da cui dipendono.

Questa operazione è particolarmente utile per il cluster iperconvergente locale. Con monitoraggio di Azure integrato, sarà possibile configurare messaggi di posta elettronica, SMS e altri avvisi per eseguire il ping quando si verifica un errore nel cluster (o quando si vuole contrassegnare un'altra attività in base ai dati raccolti). Di seguito viene illustrato brevemente il funzionamento di monitoraggio di Azure, come installare monitoraggio di Azure e come configurarlo per l'invio di notifiche.

Se si usa System Center, vedere la [Spazi di archiviazione diretta Management Pack](https://www.microsoft.com/download/details.aspx?id=100782) che monitora i cluster di windows server 2019 e windows server 2016 spazi di archiviazione diretta.

Il Management Pack include le funzionalità seguenti:

* Monitoraggio dell'integrità e delle prestazioni del disco fisico
* Monitoraggio dello stato e delle prestazioni del nodo di archiviazione
* Monitoraggio dello stato e delle prestazioni del pool di archiviazione
* Tipo di resilienza del volume e stato della deduplicazione

## <a name="understanding-azure-monitor"></a>Informazioni su monitoraggio di Azure

Tutti i dati raccolti da monitoraggio di Azure si integrano in uno dei due tipi fondamentali: metriche e log.

1. Le [metriche](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics) sono valori numerici che descrivono alcuni aspetti di un sistema in un determinato momento. Sono semplici e in grado di supportare scenari in tempo quasi reale. I dati raccolti da monitoraggio di Azure verranno visualizzati direttamente nella pagina panoramica nel portale di Azure.

![immagine dell'inserimento di metriche in Esplora metriche](media/configure-azure-monitor/metrics.png)

2. I [log](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#logs) contengono tipi diversi di dati organizzati in record con diversi set di proprietà per ogni tipo. I dati di telemetria, ad esempio eventi e tracce, vengono archiviati come log oltre ai dati sulle prestazioni, in modo che possano essere combinati per l'analisi. I dati di log raccolti da monitoraggio di Azure possono essere analizzati con [query](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) per recuperare, consolidare e analizzare rapidamente i dati raccolti. È possibile creare e testare le query usando [log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/portals) nel portale di Azure e quindi analizzare direttamente i dati usando questi strumenti o salvare le query da usare con [visualizzazioni](https://docs.microsoft.com/azure/azure-monitor/visualizations) o [regole di avviso](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

![immagine dell'inserimento dei log in log Analytics](media/configure-azure-monitor/logs.png)

Di seguito sono riportati altri dettagli su come configurare questi avvisi.

## <a name="onboarding-your-cluster-using-windows-admin-center"></a>Onboarding del cluster con l'interfaccia di amministrazione di Windows

Usando l'interfaccia di amministrazione di Windows, è possibile caricare il cluster in monitoraggio di Azure.

![Gif del cluster di onboarding in monitoraggio di Azure "](media/configure-azure-monitor/onboarding.gif)

Durante questo flusso di onboarding, i passaggi seguenti si verificano dietro le quinte. Viene illustrato in dettaglio come configurarli in dettaglio nel caso in cui si desideri configurare manualmente il cluster. 

### <a name="configuring-health-service"></a>Configurazione di Servizio integrità

Per prima cosa è necessario configurare il cluster. Come è noto, il [servizio integrità](../../failover-clustering/health-service-overview.md) migliora l'esperienza operativa e di monitoraggio quotidiana per i cluster che eseguono spazi di archiviazione diretta. 

Come illustrato in precedenza, monitoraggio di Azure raccoglie i log da ogni nodo su cui è in esecuzione nel cluster. Quindi, dobbiamo configurare il Servizio integrità per scrivere in un canale di eventi, che è il seguente:

```
Event Channel: Microsoft-Windows-Health/Operational
Event ID: 8465
```

Per configurare la Servizio integrità, eseguire:

```PowerShell
get-storagesubsystem clus* | Set-StorageHealthSetting -Name "Platform.ETW.MasTypes" -Value "Microsoft.Health.EntityType.Subsystem,Microsoft.Health.EntityType.Server,Microsoft.Health.EntityType.PhysicalDisk,Microsoft.Health.EntityType.StoragePool,Microsoft.Health.EntityType.Volume,Microsoft.Health.EntityType.Cluster"
```

Quando si esegue il cmdlet precedente per impostare le impostazioni di integrità, si determinano gli eventi che si desidera vengano scritti nel canale di eventi *Microsoft-Windows-Health/Operational* .

### <a name="configuring-log-analytics"></a>Configurazione di Log Analytics

Ora che è stata impostata la registrazione corretta nel cluster, il passaggio successivo consiste nel configurare correttamente log Analytics.

Per offrire una panoramica, [Azure log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows) può raccogliere i dati direttamente dai computer fisici o virtuali Windows nel Data Center o in un altro ambiente cloud in un unico repository per l'analisi e la correlazione dettagliate.

Per comprendere la configurazione supportata, esaminare i [sistemi operativi Windows supportati](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) e la [configurazione del firewall di rete](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

#### <a name="login-in-to-azure-portal"></a>Accedi al portale di Azure

Accedere al portale di Azure in [https://portal.azure.com](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

#### <a name="create-a-workspace"></a>Crea area di lavoro

Per altri dettagli sui passaggi elencati di seguito, vedere la [documentazione di monitoraggio di Azure](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-windows-computer).

1. Nella portale di Azure fare clic su **tutti i servizi**. Nell'elenco di risorse digitare **log Analytics**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **log Analytics**.<br><br> 

   ![Portale di Azure](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. Fare clic su **Crea**e quindi selezionare le opzioni per gli elementi seguenti:

   * Consente di specificare un nome per la nuova **area di lavoro log Analytics**, ad esempio *DefaultLAWorkspace*. 
   * Selezionare una **sottoscrizione** a cui collegarsi selezionando nell'elenco a discesa se l'impostazione predefinita selezionata non è appropriata.
   * Per **gruppo di risorse**selezionare un gruppo di risorse esistente contenente una o più macchine virtuali di Azure. <br><br>

      ![Pannello Crea risorsa Log Analytics](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. Dopo aver fornito le informazioni necessarie nel riquadro **area di lavoro log Analytics** , fare clic su **OK**.  

Mentre le informazioni vengono verificate e viene creata l'area di lavoro, è possibile tenere traccia dello stato di avanzamento in **notifiche** dal menu. 

#### <a name="obtain-workspace-id-and-key"></a>Ottenere la chiave e l'ID dell'area di lavoro
Prima di installare il Microsoft Monitoring Agent per Windows, sono necessari l'ID e la chiave dell'area di lavoro per l'area di lavoro di Log Analytics.  Queste informazioni sono richieste dall'installazione guidata per configurare correttamente l'agente e verificare che possa comunicare correttamente con Log Analytics.  

1. Nella portale di Azure fare clic su **tutti i servizi** presenti nell'angolo superiore sinistro. Nell'elenco di risorse digitare **log Analytics**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **log Analytics**.
2. Nell'elenco delle aree di lavoro Log Analytics selezionare *DefaultLAWorkspace* creato in precedenza.
3. Selezionare **Impostazioni avanzate**.<br><br> ![Log Analytics impostazioni avanzate](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. Selezionare **origini connesse**e quindi selezionare **server Windows**.   
5. Valore a destra di **ID area di lavoro** e **chiave primaria**. Salvare sia temporaneamente che copiare e incollare entrambi nell'editor preferito per il momento.   

### <a name="installing-the-agent-on-windows"></a>Installazione dell'agente in Windows
I passaggi seguenti installano e configurano il Microsoft Monitoring Agent. **Assicurarsi di installare questo agente in ogni server del cluster e indicare che si desidera che l'agente venga eseguito all'avvio di Windows.**

1. Nella pagina **server Windows** selezionare la versione appropriata per scaricare l' **agente Windows** da scaricare, a seconda dell'architettura del processore del sistema operativo Windows.
2. Eseguire il programma di installazione per installare l'agente nel computer.
2. Nella pagina inizialefare clic su **Avanti**.
3. Nella pagina **condizioni di licenza** leggere la licenza e **quindi fare clic su Accetto.**
4. Nella pagina **cartella di destinazione** modificare o salvare la cartella di installazione predefinita e quindi fare clic su **Avanti**.
5. Nella pagina **Opzioni di installazione dell'agente** scegliere di connettere l'agente ad Azure log Analytics e quindi fare clic su **Avanti**.   
6. Nella pagina **log Analytics di Azure** eseguire le operazioni seguenti:
   1. Incollare l' **ID area** di lavoro e la **chiave dell'area di lavoro (chiave primaria)** copiati in precedenza.    
    a. Se il computer deve comunicare tramite un server proxy al servizio Log Analytics, fare clic su **Avanzate** e specificare l'URL e il numero di porta del server proxy.  Se il server proxy richiede l'autenticazione, digitare il nome utente e la password per l'autenticazione con il server proxy e quindi fare clic su **Avanti**.  
7. Al termine, fare clic su **Avanti** per fornire le impostazioni di configurazione necessarie.<br><br> ![incollare l'ID area di lavoro e la chiave primaria](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Nella pagina **pronto per l'installazione** , rivedere le scelte effettuate e quindi fare clic su **Installa**.
9. Nella pagina **Configurazione completata** fare clic su **fine**.

Al termine, il **Microsoft Monitoring Agent** verrà visualizzato nel **Pannello di controllo**. È possibile rivedere la configurazione e verificare che l'agente sia connesso a Log Analytics. Quando si è connessi, nella scheda **log Analytics di Azure** l'agente Visualizza un messaggio che informa **che la Microsoft Monitoring Agent è stata connessa correttamente al servizio Microsoft log Analytics.** 

![Stato connessione MMA a Log Analytics](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

Per comprendere la configurazione supportata, esaminare i [sistemi operativi Windows supportati](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) e la [configurazione del firewall di rete](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

## <a name="setting-up-alerts-using-windows-admin-center"></a>Impostazione degli avvisi tramite l'interfaccia di amministrazione di Windows

Nell'interfaccia di amministrazione di Windows è possibile configurare gli avvisi predefiniti che verranno applicati a tutti i server nell'area di lavoro Log Analytics. 

![Gif della configurazione degli avvisi](media/configure-azure-monitor/setup1.gif)

Questi sono gli avvisi e le relative condizioni predefinite che è possibile acconsentire esplicitamente:

| Nome dell'avviso                | Condizione predefinita                                  |
|---------------------------|----------------------------------------------------|
| Utilizzo CPU           | Oltre il 85% per 10 minuti                            |
| Utilizzo della capacità del disco | Oltre il 85% per 10 minuti                            |
| Utilizzo memoria        | Memoria disponibile inferiore a 100 MB per 10 minuti   |
| Heartbeat                 | Meno di 2 battimenti per 5 minuti                   |
| Errore critico di sistema     | Qualsiasi avviso critico nel registro eventi di sistema del cluster |
| Avviso servizio integrità      | Qualsiasi errore del servizio integrità nel cluster            |

Dopo aver configurato gli avvisi nell'interfaccia di amministrazione di Windows, è possibile visualizzare gli avvisi nell'area di lavoro di log Analytics in Azure.

![Gif della configurazione degli avvisi](media/configure-azure-monitor/setup2.gif)

Durante questo flusso di onboarding, i passaggi seguenti si verificano dietro le quinte. Viene illustrato in dettaglio come configurarli in dettaglio nel caso in cui si desideri configurare manualmente il cluster. 

### <a name="collecting-event-and-performance-data"></a>Raccolta di dati di eventi e prestazioni

Log Analytics possibile raccogliere gli eventi dal registro eventi di Windows e dai contatori delle prestazioni specificati per l'analisi a lungo termine e la creazione di report e intervenire quando viene rilevata una determinata condizione.  Seguire questi passaggi per configurare la raccolta di eventi dal registro eventi di Windows e alcuni contatori delle prestazioni comuni per iniziare con.  

1. Nella portale di Azure fare clic su **altri servizi** nell'angolo in basso a sinistra. Nell'elenco di risorse digitare **log Analytics**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **log Analytics**.
2. Selezionare **Impostazioni avanzate**.<br><br> ![Log Analytics impostazioni avanzate](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. Selezionare **dati**, quindi selezionare **registri eventi di Windows**.  
4. A questo punto, aggiungere il canale dell'evento Servizio integrità digitando il nome seguente e facendo clic sul segno più **+** .  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. Nella tabella controllare l' **errore** di gravità e l' **avviso**.   
6. Fare clic su **Salva** nella parte superiore della pagina per salvare la configurazione.
7. Selezionare i **contatori delle prestazioni di Windows** per abilitare la raccolta dei contatori delle prestazioni in un computer Windows. 
8. Quando si configurano i contatori delle prestazioni di Windows per la prima volta per una nuova area di lavoro Log Analytics, è possibile creare rapidamente diversi contatori comuni. Sono elencate con una casella di controllo accanto a ognuna.<br> ![i contatori delle prestazioni di Windows predefiniti selezionati](media/configure-azure-monitor/windows-perfcounters-default.png)<br> Fare clic su **Aggiungi i contatori delle prestazioni selezionati**.  Vengono aggiunti e preimpostati con un intervallo di campionamento della raccolta di dieci secondi.  
9. Fare clic su **Salva** nella parte superiore della pagina per salvare la configurazione.

## <a name="creating-alerts-based-on-log-data"></a>Creazione di avvisi in base ai dati di log

In questo modo, il cluster deve inviare i log e i contatori delle prestazioni a Log Analytics. Il passaggio successivo consiste nel creare regole di avviso che eseguono automaticamente ricerche log a intervalli regolari. Se i risultati della ricerca log corrispondono a criteri specifici, viene generato un avviso che invia un messaggio di posta elettronica o una notifica di testo. Di seguito viene illustrato il problema.

### <a name="create-a-query"></a>Creare una query

Per iniziare, aprire il portale per la ricerca log.   

1. Nella portale di Azure fare clic su **tutti i servizi**. Nell'elenco di risorse digitare **Monitor**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **monitoraggio**.
2. Nel menu di spostamento del monitoraggio selezionare **log Analytics** e quindi selezionare un'area di lavoro.

Il modo più rapido per recuperare alcuni dati da usare è una query semplice che restituisce tutti i record nella tabella. Digitare le query seguenti nella casella di ricerca e fare clic sul pulsante Cerca.  

```
Event
```

I dati vengono restituiti nella visualizzazione elenco predefinita ed è possibile visualizzare il numero totale di record restituiti.

![Query semplice](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

Sul lato sinistro della schermata è presente il riquadro filtro che consente di aggiungere filtri alla query senza modificarla direttamente.  Per il tipo di record vengono visualizzate diverse proprietà dei record ed è possibile selezionare uno o più valori di proprietà per restringere i risultati della ricerca.

Selezionare la casella di controllo accanto a **errore** in **EVENTLEVELNAME** o digitare quanto segue per limitare i risultati agli eventi di errore.

```
Event | where (EventLevelName == "Error")
```

![Filtro](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

Dopo aver eseguito le query approriate per gli eventi a cui si è interessati, salvarli per il passaggio successivo.

### <a name="create-alerts"></a>Creazione di avvisi
Esaminiamo ora un esempio per la creazione di un avviso.

1. Nella portale di Azure fare clic su **tutti i servizi**. Nell'elenco di risorse digitare **log Analytics**. Non appena si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **log Analytics**.
2. Nel riquadro a sinistra selezionare **avvisi** , quindi fare clic su **nuova regola di avviso** nella parte superiore della pagina per creare un nuovo avviso.<br><br> ![creare una nuova regola di avviso](media/configure-azure-monitor/alert-rule-02.png)<br>
3. Per il primo passaggio, nella sezione **Crea avviso** si selezionerà l'area di lavoro log Analytics come risorsa, poiché si tratta di un segnale di avviso basato su log.  Filtrare i risultati scegliendo la **sottoscrizione** specifica dall'elenco a discesa se ne sono presenti più di uno, che contiene log Analytics area di lavoro creata in precedenza.  Filtrare il **tipo di risorsa** selezionando **log Analytics** dall'elenco a discesa.  Infine, selezionare la **risorsa** **DefaultLAWorkspace** , quindi fare clic su **fine**.<br><br> ![creare un avviso passaggio 1 attività](media/configure-azure-monitor/alert-rule-03.png)<br>
4. Nella sezione **criteri di avviso**, fare clic su **Aggiungi criteri** per selezionare la query salvata e quindi specificare la logica che la regola di avviso segue.
5. Configurare l'avviso con le seguenti informazioni:  
   a. Dall'elenco **a discesa basato su** selezionare **misurazione metrica**.  Una misura metrica creerà un avviso per ogni oggetto nella query con un valore che supera la soglia specificata.  
   b. Per la **condizione**selezionare **maggiore di** e specificare un thershold.  
   c. Definire quindi quando attivare l'avviso. È ad esempio possibile selezionare **violazioni consecutive** e nell'elenco a discesa selezionare un valore **maggiore** di 3.  
   d. In valutazione basata sulla sezione, modificare il valore del **periodo** su **30** minuti e **frequenza** su 5. La regola viene eseguita ogni cinque minuti e restituisce i record creati negli ultimi trenta minuti dall'ora corrente.  Se si imposta il periodo di tempo su una finestra più ampia per la potenziale latenza dei dati, e si garantisce che la query restituisca dati per evitare un falso negativo in cui l'avviso non viene mai attivato.  
6. Fare clic su **fine** per completare la regola di avviso.<br><br> ![configurare il segnale di avviso](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. A questo punto, passare al secondo passaggio, specificare un nome per l'avviso nel campo **Nome regola di avviso** , ad esempio **avviso per tutti gli eventi di errore**.  Specificare una **Descrizione** che detaili le specifiche per l'avviso e selezionare **Critical (gravità 0)** per il valore di **gravità** dalle opzioni fornite.
8. Per attivare immediatamente la regola di avviso durante la creazione, accettare il valore predefinito per **Abilita regola al momento della creazione**.
9. Per il terzo e ultimo passaggio, si specifica un **gruppo di azione**che garantisce che vengano eseguite le stesse azioni ogni volta che viene attivato un avviso e che può essere usato per ogni regola definita. Configurare un nuovo gruppo di azione con le seguenti informazioni:  
   a. Selezionare **nuovo gruppo di azioni** e verrà visualizzato il riquadro **Aggiungi gruppo di azioni** .  
   b. Per **nome gruppo di azioni**specificare un nome, ad **esempio operazioni IT-notifica** e un **nome breve** , ad esempio **ITOPS-n**.  
   c. Verificare che i valori predefiniti per la **sottoscrizione** e il **gruppo di risorse** siano corretti. In caso contrario, selezionare quella corretta dall'elenco a discesa.   
   d. Nella sezione Azioni specificare un nome per l'azione, ad esempio **Invia messaggio di posta elettronica** e in **tipo di azione** Selezionare **posta elettronica/SMS/push/voce** dall'elenco a discesa. Il riquadro delle proprietà **posta elettronica/SMS/push/voce** si aprirà a destra per fornire informazioni aggiuntive.  
   e. Nel riquadro **e-mail/SMS/push/Voice** selezionare e configurare le preferenze. Ad esempio, abilitare la **posta elettronica** e fornire un indirizzo SMTP di posta elettronica valido a cui recapitare il messaggio.  
   f. Fare clic su **OK** per salvare le modifiche.<br><br> 

    ![Crea nuovo gruppo di azioni](media/configure-azure-monitor/action-group-properties-01.png)

10. Fare clic su **OK** per completare il gruppo di azioni. 
11. Fare clic su **Crea regola di avviso** per completare la regola di avviso. L'esecuzione viene avviata immediatamente.<br><br> ![completare la creazione della nuova regola di avviso](media/configure-azure-monitor/alert-rule-01.png)<br> 

### <a name="example-alert"></a>Avviso di esempio

Come riferimento, questo è l'aspetto di un avviso di esempio in Azure.

![Gif dell'avviso in Azure](media/configure-azure-monitor/alert.gif)

Di seguito è riportato un esempio del messaggio di posta elettronica che verrà inviato da monitoraggio di Azure:

![Esempio di messaggio di posta elettronica di avviso](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>Vedere anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- Per informazioni più dettagliate, vedere la [documentazione di monitoraggio di Azure](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata).
- Per una panoramica su come [connettersi ad altri servizi ibridi di Azure](../../manage/windows-admin-center/azure/index.md), vedere questo articolo.
