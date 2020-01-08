---
title: Informazioni e configurazione di monitoraggio di Azure
description: Informazioni dettagliate sulla configurazione di monitoraggio di Azure e su come configurare gli avvisi di posta elettronica e SMS per il cluster con spazi di archiviazione diretta in Windows Server 2016 e 2019.
keywords: Spazi di archiviazione diretta, monitoraggio di Azure, notifiche, posta elettronica, SMS
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 4a11ad670bdd26cdc771bb5ae357db4928995bb8
ms.sourcegitcommit: bfe9c5f7141f4f2343a4edf432856f07db1410aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75352615"
---
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>Usare monitoraggio di Azure per inviare messaggi di posta elettronica per gli errori di Servizio integrità

>Si applica a: Windows Server 2019, Windows Server 2016

Monitoraggio di Azure ottimizza la disponibilità e le prestazioni delle applicazioni in uso offrendo una soluzione completa per raccogliere e analizzare la telemetria e intervenire di conseguenza dal cloud e dagli ambienti locali. È utile per ottenere informazioni sulle prestazioni delle applicazioni e identificare in modo proattivo i problemi delle applicazioni e delle risorse da cui dipendono.

Questa operazione è particolarmente utile per il cluster iperconvergente locale. Con monitoraggio di Azure integrato, sarà possibile configurare messaggi di posta elettronica, SMS e altri avvisi per eseguire il ping quando si verifica un errore nel cluster (o quando si vuole contrassegnare un'altra attività in base ai dati raccolti). Di seguito viene illustrato brevemente il funzionamento di monitoraggio di Azure, come installare monitoraggio di Azure e come configurarlo per l'invio di notifiche.


## <a name="understanding-azure-monitor"></a>Informazioni su monitoraggio di Azure

Tutti i dati raccolti da monitoraggio di Azure si integrano in uno dei due tipi fondamentali: metriche e log.

1. Le [metriche](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics) sono valori numerici che descrivono alcuni aspetti di un sistema in un particolare momento. Sono elementi leggeri in grado di supportare scenari praticamente in tempo reale. I dati raccolti da monitoraggio di Azure verranno visualizzati direttamente nella pagina panoramica nel portale di Azure.

![immagine dell'inserimento di metriche in Esplora metriche](media/configure-azure-monitor/metrics.png)

2. I [log](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#logs) contengono diversi tipi di dati organizzati in record con diversi set di proprietà per ogni tipo. I dati di telemetria, come eventi e tracce, vengono archiviati come log insieme ai dati sulle prestazioni in modo da poter essere combinati per l'analisi. I dati di log raccolti da Monitoraggio di Azure possono essere analizzati con [query](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) per recuperare, consolidare e analizzare rapidamente i dati raccolti. È possibile creare e testare query usando [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/portals) nel portale di Azure e quindi analizzare direttamente i dati usando questi strumenti oppure salvare le query per usarle con [visualizzazioni](https://docs.microsoft.com/azure/azure-monitor/visualizations) o [regole degli avvisi](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

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

Per comprendere la configurazione supportata, vedere i [sistemi operativi Windows supportati](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) e la [configurazione del firewall di rete](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

Se non si ha una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

#### <a name="login-in-to-azure-portal"></a>Accedi al portale di Azure

Accedere al portale di Azure all'indirizzo [https://portal.azure.com](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

#### <a name="create-a-workspace"></a>Crea area di lavoro

Per altri dettagli sui passaggi elencati di seguito, vedere la [documentazione di monitoraggio di Azure](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-windows-computer).

1. Nel portale di Azure fare clic su **Tutti i servizi**. Nell'elenco delle risorse digitare **Log Analytics**. Quando si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Log Analytics**.<br><br> 

   ![Portale di Azure](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. Fare clic su **Crea** e quindi selezionare le opzioni per gli elementi seguenti:

   * Specificare un nome per la nuova **area di lavoro Log Analytics Workspace**, ad esempio *DefaultLAWorkspace*. 
   * Selezionare una **sottoscrizione** a cui collegarsi. Se la sottoscrizione selezionata per impostazione predefinita non è appropriata, è possibile sceglierne una dall'elenco a discesa.
   * Per **Gruppo di risorse**, selezionare un gruppo di risorse esistente contenente una o più macchine virtuali di Azure. <br><br>

      ![Creare il pannello della risorsa Log Analytics](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. Dopo aver specificato le informazioni necessarie nel riquadro **area di lavoro Log Analytics**, fare clic su **OK**.  

Per tenere traccia dello stato di avanzamento della verifica delle informazioni e della creazione dell'area di lavoro, è possibile usare la voce **Notifiche** nel menu. 

#### <a name="obtain-workspace-id-and-key"></a>Ottenere l'ID e la chiave dell'area di lavoro
Prima di installare Microsoft Monitoring Agent per Windows, sono necessari l'ID e la chiave dell'area di lavoro per l'area di lavoro Log Analytics.  Queste informazioni sono richieste dall'installazione guidata per configurare correttamente l'agente e verificare che possa comunicare correttamente con Log Analytics.  

1. Nel portale di Azure fare clic su **Tutti i servizi** nell'angolo superiore sinistro. Nell'elenco delle risorse digitare **Log Analytics**. Quando si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Log Analytics**.
2. Nell'elenco di aree di lavoro di Log Analytics selezionare *DefaultLAWorkspace* creata prima.
3. Selezionare **Impostazioni avanzate**.<br><br> ![Impostazioni avanzate di Log Analytics](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. Selezionare **Origini connesse** e quindi **Server Windows**.   
5. Il valore a destra di **ID area di lavoro** e **Chiave primaria**. Salvare sia temporaneamente che copiare e incollare entrambi nell'editor preferito per il momento.   

### <a name="installing-the-agent-on-windows"></a>Installazione dell'agente in Windows
I passaggi seguenti installano e configurano il Microsoft Monitoring Agent. **Assicurarsi di installare questo agente in ogni server del cluster e indicare che si desidera che l'agente venga eseguito all'avvio di Windows.**

1. Nella pagina **Server Windows** selezionare la versione appropriata da scaricare in **Scarica agente Windows** a seconda dell'architettura del processore del sistema operativo Windows.
2. Eseguire il programma di installazione per installare l'agente nel computer in uso.
2. Nella pagina **Installazione guidata** fare clic sul pulsante **Avanti**.
3. Nella pagina **Condizioni di licenza** leggere la licenza e quindi fare clic su **Accetto**.
4. Nella pagina **Cartella di destinazione** modificare o mantenere la cartella di installazione predefinita e quindi fare clic su **Avanti**.
5. Nella pagina **Opzioni di installazione dell'agente** scegliere di connettere l'agente ad Azure Log Analytics e quindi fare clic su **Avanti**.   
6. Nella pagina **Azure Log Analytics** eseguire le operazioni seguenti:
   1. Incollare **ID area di lavoro** e **Chiave dell'area di lavoro (Chiave primaria)** copiati in precedenza.    
    a. Se il computer deve comunicare tramite un server proxy con il servizio Log Analytics, fare clic su **Avanzate** e specificare l'URL e il numero di porta del server proxy.  Se il server proxy richiede l'autenticazione, digitare il nome utente e la password per l'autenticazione nel server proxy, quindi fare clic su **Avanti**.  
7. Fare clic su **Avanti** dopo aver specificato le impostazioni di configurazione necessarie.<br><br> ![incollare ID area di lavoro e chiave primaria](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Nella pagina **Pronto per l'installazione** rivedere le scelte effettuate e quindi fare clic su **Installa**.
9. Nella pagina **Configurazione completata** fare clic su **Fine**.

Al termine, l' **Microsoft Monitoring Agent** viene visualizzato nel **Pannello di controllo**. È possibile rivedere la configurazione e verificare che l'agente sia connesso a Log Analytics. Quando si è connessi, nella scheda **Azure Log Analytics** l'agente visualizza un messaggio che indica che **Microsoft Monitoring Agent ha eseguito la connessione al servizio Microsoft Log Analytics**. 

![Stato della connessione di MMA a Log Analytics](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

Per comprendere la configurazione supportata, vedere i [sistemi operativi Windows supportati](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) e la [configurazione del firewall di rete](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

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

Log Analytics può raccogliere gli eventi dai registri eventi e dai contatori delle prestazioni di Windows specificati per l'analisi a lungo termine e la creazione di report e intervenire quando viene rilevata una particolare condizione.  Seguire questi passaggi per configurare la raccolta di eventi dal registro eventi di Windows e di diversi contatori delle prestazioni comuni con cui iniziare.  

1. Nel portale di Azure fare clic su **Altri servizi** nell'angolo in basso a sinistra. Nell'elenco delle risorse digitare **Log Analytics**. Quando si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Log Analytics**.
2. Selezionare **Impostazioni avanzate**.<br><br> ![Impostazioni avanzate di Log Analytics](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. Selezionare **Dati** e quindi selezionare **Log eventi Windows**.  
4. A questo punto, aggiungere il canale dell'evento Servizio integrità digitando il nome seguente e facendo clic sul segno più **+** .  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. Nella tabella selezionare i livelli di gravità **Errore** e **Avviso**.   
6. Fare clic su **Salva** nella parte superiore della pagina per salvare la configurazione.
7. Selezionare **Contatori delle prestazioni di Windows** per abilitare la raccolta di contatori delle prestazioni in un computer Windows. 
8. Quando si configurano i contatori delle prestazioni di Windows per la prima volta per una nuova area di lavoro Log Analytics, è possibile creare rapidamente numerosi contatori comuni. Viene visualizzato l'elenco dei contatori con le caselle di controllo corrispondenti.<br> ![Contatori delle prestazioni di Windows predefiniti selezionati](media/configure-azure-monitor/windows-perfcounters-default.png)<br> Fare clic su **Aggiungi i contatori delle prestazioni selezionati**.  Vengono aggiunti e preimpostati con un intervallo di esempio tra le raccolte di dieci secondi.  
9. Fare clic su **Salva** nella parte superiore della pagina per salvare la configurazione.

## <a name="creating-alerts-based-on-log-data"></a>Creazione di avvisi in base ai dati di log

In questo modo, il cluster deve inviare i log e i contatori delle prestazioni a Log Analytics. Il passaggio successivo consiste nel creare regole di avviso che eseguono automaticamente ricerche log a intervalli regolari. Se i risultati della ricerca log corrispondono a criteri specifici, viene generato un avviso che invia un messaggio di posta elettronica o una notifica di testo. Di seguito viene illustrato il problema.

### <a name="create-a-query"></a>Creare una query

Per iniziare, aprire il portale per la ricerca log.   

1. Nel portale di Azure fare clic su **Tutti i servizi**. Nell'elenco delle risorse digitare **Monitoraggio**. Quando si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Monitoraggio**.
2. Nel menu di spostamento Monitoraggio scegliere **Log Analytics** e quindi selezionare un'area di lavoro.

Il modo più rapido per recuperare alcuni dati da utilizzare è una query semplice che restituisce tutti i record in una tabella. Digitare le query seguenti nella casella di ricerca e fare clic sul pulsante Cerca.  

```
Event
```

I dati vengono restituiti nella visualizzazione elenco predefinita ed è possibile vedere il numero totale di record restituiti.

![Query semplice](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

Sul lato sinistro della schermata è disponibile il riquadro di filtro che consente di aggiungere filtri alla query senza modificarla direttamente.  Vengono visualizzate diverse proprietà di record per tale tipo di record ed è possibile selezionare uno o più valori di proprietà per restringere i risultati di ricerca.

Selezionare la casella di controllo accanto a **errore** in **EVENTLEVELNAME** o digitare quanto segue per limitare i risultati agli eventi di errore.

```
Event | where (EventLevelName == "Error")
```

![Filter](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

Dopo aver eseguito le query approriate per gli eventi a cui si è interessati, salvarli per il passaggio successivo.

### <a name="create-alerts"></a>Creare avvisi
Esaminiamo ora un esempio per la creazione di un avviso.

1. Nel portale di Azure fare clic su **Tutti i servizi**. Nell'elenco delle risorse digitare **Log Analytics**. Quando si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Log Analytics**.
2. Nel riquadro sinistro selezionare **Avvisi** e quindi fare clic su **Nuova regola di avviso** all'inizio della pagina per creare un nuovo avviso.<br><br> ![Creazione di una nuova regola di avviso](media/configure-azure-monitor/alert-rule-02.png)<br>
3. Per il primo passaggio, nella sezione **Crea avviso** occorre selezionare l'area di lavoro Log Analytics come risorsa, poiché di tratta di un segnale di avviso basato su log.  Filtrare i risultati scegliendo la **sottoscrizione** specifica dall'elenco a discesa se ne sono presenti più di uno, che contiene log Analytics area di lavoro creata in precedenza.  Filtrare **Tipo di risorsa** selezionando **Log Analytics** nell'elenco a discesa.  Infine, selezionare la **risorsa** **DefaultLAWorkspace** , quindi fare clic su **fine**.<br><br> ![Passaggio 1 della creazione di un avviso](media/configure-azure-monitor/alert-rule-03.png)<br>
4. Nella sezione **criteri di avviso**, fare clic su **Aggiungi criteri** per selezionare la query salvata e quindi specificare la logica che la regola di avviso segue.
5. Configurare l'avviso con le informazioni seguenti:  
   a. Nell'elenco a discesa **In base a** selezionare **Unità di misura della metrica**.  Un'unità di misura della metrica creerà un avviso per ogni oggetto nella query con un valore che supera la soglia specificata.  
   b. Per la **condizione**selezionare **maggiore di** e specificare un thershold.  
   c. Definire quindi quando attivare l'avviso. È ad esempio possibile selezionare **violazioni consecutive** e nell'elenco a discesa selezionare un valore **maggiore** di 3.  
   d. In valutazione basata sulla sezione, modificare il valore del **periodo** su **30** minuti e **frequenza** su 5. La regola verrà eseguita ogni cinque minuti e restituirà i record creati negli ultimi 30 minuti a partire dall'ora corrente.  Se si imposta il periodo di tempo su una finestra più ampia, si tiene conto del potenziale di latenza dei dati e si garantisce che la query restituisca dati per evitare un falso negativo in cui l'avviso non viene mai visualizzato.  
6. Fare clic su **Fine** per completare la regola di avviso.<br><br> ![Configurazione del segnale di avviso](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. A questo punto, passare al secondo passaggio, specificare un nome per l'avviso nel campo **Nome regola di avviso** , ad esempio **avviso per tutti gli eventi di errore**.  In **Descrizione** specificare i dettagli dell'avviso e selezionare **Critico (gravità 0)** come valore di **Gravità** nelle opzioni disponibili.
8. Per attivare immediatamente la regola di avviso alla creazione, accettare il valore predefinito di **Abilita regola alla creazione**.
9. Per il terzo e ultimo passaggio occorre specificare un **gruppo di azioni**, che assicura che vengano eseguite le stesse azioni ogni volta che viene attivato un avviso e che può essere usato per ogni regola definita. Configurare un nuovo gruppo di azioni con le informazioni seguenti:  
   a. Selezionare **Nuovo gruppo di azioni** per visualizzare il riquadro **Aggiungi gruppo di azioni**.  
   b. In **Nome gruppo di azioni** specificare un nome come **Operazioni IT - Notifica** e in **Nome breve** specificare un nome come **itops-n**.  
   c. Verificare che i valori predefiniti di **Sottoscrizione** e **Gruppo di risorse** siano corretti. Se non lo sono, selezionare i valori corretti negli elenchi a discesa.   
   d. Nella sezione Azioni specificare un nome per l'azione, ad esempio **Invio di posta elettronica**, e in **Tipo di azione** selezionare **Posta elettronica/SMS/Push/Voce** nell'elenco a discesa. Il riquadro delle proprietà **Posta elettronica/SMS/Push/Voce** si apre sulla destra per fornire informazioni aggiuntive.  
   e. Nel riquadro **e-mail/SMS/push/Voice** selezionare e configurare le preferenze. Ad esempio, abilitare la **posta elettronica** e fornire un indirizzo SMTP di posta elettronica valido a cui recapitare il messaggio.  
   f. Fare clic su **OK** per salvare le modifiche.<br><br> 

    ![Creare un nuovo gruppo di azioni](media/configure-azure-monitor/action-group-properties-01.png)

10. Fare clic su **OK** per completare la creazione del gruppo di azioni. 
11. Fare clic su **Crea regola di avviso** per completare la creazione della regola di avviso. L'esecuzione inizia immediatamente.<br><br> ![Completamento della creazione della nuova regola di avviso](media/configure-azure-monitor/alert-rule-01.png)<br> 

## <a name="see-alerts"></a>Visualizza avvisi

Come riferimento, questo è l'aspetto di un avviso di esempio in Azure.

![Gif dell'avviso in Azure "](media/configure-azure-monitor/alert.gif)

Di seguito è riportato un esempio del messaggio di posta elettronica che verrà inviato da monitoraggio di Azure:

![Esempio di messaggio di posta elettronica di avviso](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>Vedi anche

- [Panoramica di Spazi di archiviazione diretta](storage-spaces-direct-overview.md)
- Per informazioni più dettagliate, vedere la [documentazione di monitoraggio di Azure](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata).
- Per una panoramica su come [connettersi ad altri servizi ibridi di Azure](../../manage/windows-admin-center/azure/index.md), vedere questo articolo.
