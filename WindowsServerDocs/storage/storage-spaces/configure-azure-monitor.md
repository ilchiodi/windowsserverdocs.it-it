---
title: Descrizione e configurazione di monitoraggio di Azure
description: Il programma di installazione informazioni dettagliate sulle novità di monitoraggio di Azure e su come configurare gli avvisi di posta elettronica e sms per cluster spazi di archiviazione diretta in Windows Server 2016 e 2019.
keywords: Spazi di archiviazione diretta, monitoraggio di azure, le notifiche, messaggio di posta elettronica, sms
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 908e4a7a75606905caebfa4b79168b3976982e6d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447590"
---
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>Usare monitoraggio di Azure per inviare messaggi di posta elettronica per gli errori del servizio integrità

>Si applica a: Windows Server 2019, Windows Server 2016

Monitoraggio di Azure ottimizza la disponibilità e prestazioni delle applicazioni grazie a una soluzione completa per la raccolta, analisi e agisce sui dati di telemetria dal cloud e negli ambienti locali. Consente di comprendere come le applicazioni eseguono e identifica in modo proattivo i problemi che influiscono su essi e le risorse che cui dipendono.

Ciò è particolarmente utile per il cluster iperconvergente in locale. Monitoraggio di Azure integrato, sarà possibile configurare posta elettronica, il testo (SMS) e gli altri avvisi per effettuare il ping quando si verificano problemi con il cluster (o quando si desidera contrassegnare un'altra attività basata sui dati raccolti). Di seguito, brevemente spiegheremo come monitoraggio di Azure funziona, sull'installazione di monitoraggio di Azure e come configurarlo per l'invio di notifiche.


## <a name="understanding-azure-monitor"></a>La comprensione di monitoraggio di Azure

Tutti i dati raccolti da monitoraggio di Azure rientra in uno dei due tipi fondamentali: metriche e log.

1. [Le metriche](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/data-collection#metrics) sono valori numerici che descrivono alcuni aspetti di un sistema in un particolare punto nel tempo. Sono in grado di supportare scenari in tempo reale quasi e leggero. Si noterà che i dati raccolti da destra a monitoraggio di Azure nella loro pagina di panoramica nel portale di Azure.

![immagine di inserimento in blocco le metriche in Esplora metriche](media/configure-azure-monitor/metrics.png)

2. [I log](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/data-collection#logs) contengono diversi tipi di dati organizzati in record con diversi set di proprietà per ogni tipo. I dati di telemetria, ad esempio eventi e tracce vengono archiviati sotto forma di log anche per i dati sulle prestazioni in modo che si possono tutti essere combinato per l'analisi. I dati di log raccolti da monitoraggio di Azure possono essere analizzati con [query](https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/log-query-overview) recuperare, consolidare e analizzare i dati raccolti. È possibile creare e testare le query che utilizzano [Log Analitica](https://docs.microsoft.com/en-us/azure/azure-monitor/log-query/portals) nel portale di Azure e quindi analizzare direttamente i dati usando questi strumenti o salvare le query per l'uso con [visualizzazioni](https://docs.microsoft.com/en-us/azure/azure-monitor/visualizations) o [avviso le regole](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/alerts-overview).

![immagine dei log, l'inserimento di analitica di log](media/configure-azure-monitor/logs.png)

Saranno disponibili sotto per maggiori dettagli su come configurare questi avvisi.

## <a name="configuring-health-service"></a>Configurazione del servizio integrità

Prima di tutto devi fare è configurare il cluster. Come è noto, il [servizio integrità](../../failover-clustering/health-service-overview.md) migliora l'esperienza operativa per i cluster che esegue spazi di archiviazione diretta e monitoraggio quotidiano. 

Come descritto in precedenza, il monitoraggio di Azure raccoglie i log da ogni nodo da cui è in esecuzione nel cluster. Pertanto, è necessario configurare il servizio integrità in cui scrivere un canale dell'evento, che risulta essere:

```
Event Channel: Microsoft-Windows-Health/Operational
Event ID: 8465
```

Per configurare il servizio integrità, eseguire:

```PowerShell
get-storagesubsystem clus* | Set-StorageHealthSetting -Name "Platform.ETW.MasTypes" -Value "Microsoft.Health.EntityType.Subsystem,Microsoft.Health.EntityType.Server,Microsoft.Health.EntityType.PhysicalDisk,Microsoft.Health.EntityType.StoragePool,Microsoft.Health.EntityType.Volume,Microsoft.Health.EntityType.Cluster"
```

Quando si esegue il cmdlet precedente per configurare le impostazioni di integrità, è fare in modo gli eventi che si desidera iniziare su cui scrivere il *Microsoft-Windows-Health/Operational* canale dell'evento.

## <a name="configuring-log-analytics"></a>Configurazione di Log Analitica

Dopo aver creato il programma di installazione di registrazione appropriate nel cluster, il passaggio successivo consiste nel configurare correttamente log analitica.

Fornire una panoramica [Azure Log Analitica](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/agent-windows) può raccogliere i dati direttamente dai computer Windows fisico o virtuale nel tuo Data Center o un altro ambiente cloud in un unico repository per procedere a analisi dettagliate e la correlazione.

Per comprendere la configurazione supportata, esaminare [sistemi operativi Windows supportati](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) e [configurazione del firewall di rete](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

Se non hai una sottoscrizione di Azure, creare un [account gratuito](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) prima di iniziare.

### <a name="login-in-to-azure-portal"></a>Account di accesso nel portale di Azure

Accedere al portale di Azure all'indirizzo [ https://portal.azure.com ](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

### <a name="create-a-workspace"></a>Creare un'area di lavoro

Per altre informazioni sui passaggi elencati di seguito, vedere la [documentazione di monitoraggio di Azure](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/quick-collect-windows-computer).

1. Nel portale di Azure, fare clic su **tutti i servizi**. Nell'elenco delle risorse, digitare **Log Analitica**. Come si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Log Analitica**.<br><br> 

   ![Portale di Azure](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. Fare clic su **Create**e quindi selezionare le opzioni per gli elementi seguenti:

   * Specificare un nome per il nuovo **dell'area di lavoro di Log Analitica**, ad esempio *DefaultLAWorkspace*. 
   * Selezionare una **sottoscrizione** a cui collegarsi selezionando dall'elenco a discesa scegliere se il valore predefinito selezionato non è appropriato.
   * Per la **gruppo di risorse**, selezionare un gruppo di risorse esistente che contiene uno o più macchine virtuali. <br><br>

      ![Creare il pannello di risorse di Log Analitica](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. Dopo aver specificato le informazioni necessarie nel **dell'area di lavoro di Log Analitica** riquadro, fare clic su **OK**.  

Anche se le informazioni vengono verificate e viene creato l'area di lavoro, è possibile tenere traccia dello stato di avanzamento sotto **notifiche** dal menu di scelta. 

### <a name="obtain-workspace-id-and-key"></a>Ottenere l'ID dell'area di lavoro e chiave
Prima di installare l'agente per il Windows di Microsoft Monitoring, è necessario l'ID area di lavoro e la chiave dell'area di lavoro di Log Analitica.  Queste informazioni sono richieste dall'installazione guidata per configurare correttamente l'agente e verificare che possa comunicare correttamente con Log Analitica.  

1. Nel portale di Azure, fare clic su **tutti i servizi** trovato nell'angolo superiore sinistro. Nell'elenco delle risorse, digitare **Log Analitica**. Come si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Log Analitica**.
2. Nell'elenco delle aree di lavoro di Log Analitica, selezionare *DefaultLAWorkspace* creato in precedenza.
3. Selezionare **impostazioni avanzate**.<br><br> ![Impostazioni di anticipo del log Analitica](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. Selezionare **Connected Sources**, quindi selezionare **i server Windows**.   
5. Il valore a destra della **ID area di lavoro** e **Primary Key**. Salvare entrambi temporaneamente: copiare e incollare entrambi nell'editor preferito per il momento.   

## <a name="installing-the-agent-on-windows"></a>Installazione dell'agente in Windows
I seguenti passaggi installare e configurare Microsoft Monitoring Agent. **Assicurarsi di installare l'agente in ogni server del cluster e indicare che si vuole che l'agente per l'esecuzione all'avvio di Windows.**

1. Nel **Windows Server** pagina, selezionare un valore appropriato **Scarica agente Windows** versione da scaricare in base all'architettura del processore del sistema operativo Windows.
2. Eseguire il programma di installazione per installare l'agente nel computer.
2. Nella pagina **iniziale** fare clic su **Avanti**.
3. Nel **condizioni di licenza** pagina, leggere la licenza e quindi fare clic su **accetto**.
4. Nel **cartella di destinazione** pagina, modificare o mantenere la cartella di installazione predefinita e quindi fare clic su **successivo**.
5. Nel **opzioni di installazione dell'agente** pagina, scegliere di connettere l'agente ad Azure Log Analitica e quindi fare clic su **successivo**.   
6. Nel **Azure Log Analitica** pagina, effettuare le operazioni seguenti:
   1. Incollare il **ID area di lavoro** e **chiave area di lavoro (chiave primaria)** copiata in precedenza.    
    a. Se il computer deve comunicare tramite un server proxy per il servizio Log Analitica, fare clic su **avanzate** e specificare l'URL e numero di porta il server proxy.  Se il server proxy richiede l'autenticazione, digitare il nome utente e password per l'autenticazione con il server proxy e quindi fare clic su **successivo**.  
7. Fare clic su **successivo** dopo aver specificato le impostazioni di configurazione necessarie.<br><br> ![incollare ID area di lavoro e la chiave primaria](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Nel **pronto per l'installazione** pagina, rivedere le scelte effettuate e quindi fare clic su **installare**.
9. Nel **configurazione completata** pagina, fare clic su **fine**.

Al termine dell'esercitazione, il **Microsoft Monitoring Agent** viene visualizzato nella **Pannello di controllo**. È possibile rivedere la configurazione e verificare che l'agente sia connesso a Log Analitica. Quando si è connessi, scegliere il **Azure Log Analitica** scheda, l'agente Visualizza il messaggio: **Microsoft Monitoring Agent è connesso correttamente al servizio di Microsoft Log Analitica.** 

![Stato della connessione di MMA a Log Analitica](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

Per comprendere la configurazione supportata, esaminare [sistemi operativi Windows supportati](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) e [configurazione del firewall di rete](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

## <a name="collecting-event-and-performance-data"></a>La raccolta dei dati di eventi e delle prestazioni

Log Analitica può raccogliere gli eventi dal registro eventi di Windows e i contatori delle prestazioni specificati per analisi a lungo termine e creazione di report e intervenire quando viene rilevata una particolare condizione.  Seguire questi passaggi per configurare la raccolta di eventi dal registro eventi di Windows e di diversi comuni contatori delle prestazioni da cui iniziare.  

1. Nel portale di Azure, fare clic su **altri servizi** trovato nell'angolo inferiore sinistro. Nell'elenco delle risorse, digitare **Log Analitica**. Come si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Log Analitica**.
2. Selezionare **impostazioni avanzate**.<br><br> ![Impostazioni di anticipo del log Analitica](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. Selezionare **Data**, quindi selezionare **i registri eventi di Windows**.  
4. In questo caso, aggiungere il canale dell'evento servizio integrità digitando il nome seguente e quindi su segno **+** .  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. Nella tabella, controllare i livelli di gravità **errore** e **avviso**.   
6. Fare clic su **salvare** nella parte superiore della pagina per salvare la configurazione.
7. Selezionare **contatori delle prestazioni Windows** per abilitare la raccolta dei contatori delle prestazioni in un computer Windows. 
8. Quando si configurano prima di tutto i contatori delle prestazioni di Windows per una nuova area di lavoro di Log Analitica, si ha la possibilità di creare rapidamente numerosi contatori comuni. Sono elencate insieme a una casella di controllo accanto a ogni.<br> ![Contatori delle prestazioni di Windows predefiniti selezionati](media/configure-azure-monitor/windows-perfcounters-default.png)<br> Fare clic su **aggiungere i contatori delle prestazioni selezionati**.  Vengono aggiunti e preimpostati con un intervallo di campionamento di raccolte di dieci secondi.  
9. Fare clic su **salvare** nella parte superiore della pagina per salvare la configurazione.

## <a name="creating-alerts-based-on-log-data"></a>Creazione di avvisi basati sui dati di log

Se è stato impostato come questo a questo momento, il cluster deve inviando i log e contatori delle prestazioni a Log Analitica. Il passaggio successivo consiste nel creare le regole di avviso che eseguono automaticamente ricerche log a intervalli regolari. Se i risultati della ricerca log corrispondono a determinati criteri, quindi viene generato un avviso che invia una notifica di posta elettronica o SMS. È possibile esplorare queste di seguito.

### <a name="create-a-query"></a>Creare una query

Iniziare aprendo il portale di ricerca Log.   

1. Nel portale di Azure, fare clic su **tutti i servizi**. Nell'elenco delle risorse, digitare **Monitor**. Come si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Monitor**.
2. Nel menu di navigazione monitoraggio, selezionare **Log Analitica** e quindi selezionare un'area di lavoro.

Il modo più rapido per recuperare alcuni dati da utilizzare è una query semplice che restituisce tutti i record nella tabella. Digitare la query seguente nella casella di ricerca e fare clic sul pulsante di ricerca.  

```
Event
```

I dati vengono restituiti nella visualizzazione elenco predefinita ed è possibile visualizzare il numero totale di record restituiti.

![Query semplice](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

Sul lato sinistro della schermata è il riquadro di filtro che consente di aggiungere filtri alla query senza modificarla direttamente.  Vengono visualizzate diverse proprietà di record per tale tipo di record ed è possibile selezionare uno o più valori di proprietà per restringere i risultati della ricerca.

Selezionare la casella di controllo accanto a **errore** sotto **EVENTLEVELNAME** o digitare il comando seguente per limitare i risultati agli eventi di errore.

```
Event | where (EventLevelName == "Error")
```

![Filter](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

Dopo aver creato le query appropriati per gli eventi che si è interessati, salvarli per il passaggio successivo.

### <a name="create-alerts"></a>Creare avvisi
A questo punto, passiamo a esaminare un esempio per la creazione di un avviso.

1. Nel portale di Azure, fare clic su **tutti i servizi**. Nell'elenco delle risorse, digitare **Log Analitica**. Come si inizia a digitare, l'elenco viene filtrato in base all'input. Selezionare **Log Analitica**.
2. Nel riquadro di sinistra, selezionare **Alerts** e quindi fare clic su **nuova regola di avviso** nella parte superiore della pagina per creare un nuovo avviso.<br><br> ![Crea nuova regola di avviso](media/configure-azure-monitor/alert-rule-02.png)<br>
3. Per il primo passaggio, sotto il **crea avviso** sezione, si desidera selezionare l'area di lavoro di Log Analitica della risorsa, poiché si tratta di un segnale di avviso basato su log.  Filtrare i risultati scegliendo le specifiche **sottoscrizione** dall'elenco a discesa scegliere se si dispone di più di uno, che contiene l'area di lavoro di Log Analitica creato in precedenza.  Filtro il **tipo di risorsa** selezionando **Log Analitica** nell'elenco a discesa.  Selezionare infine il **Resource** **DefaultLAWorkspace** e quindi fare clic su **eseguita**.<br><br> ![Creare attività di avviso passaggio 1](media/configure-azure-monitor/alert-rule-03.png)<br>
4. Nella sezione **criteri di avviso**, fare clic su **Aggiungi criteri** per selezionare la query salvata e quindi specificare la logica che segue la regola di avviso.
5. Configurare l'avviso con le informazioni seguenti:  
   a. Dal **base** elenco a discesa, seleziona **misura della metrica**.  Una misura della metrica creerà un avviso per ogni oggetto nella query con un valore che supera la soglia specificata.  
   b. Per il **Condition**, selezionare **maggiore** e specificare un thershold.  
   c. Quindi, definire quando attivare l'avviso. Ad esempio è possibile selezionare **violazioni Consecutive** e nell'elenco a discesa selezionare **maggiore** un valore pari a 3.  
   d. Basato sulla sezione di propagazione, modificare il **periodo** valore **30** minuti e **frequenza** a 5. La regola verrà eseguita ogni cinque minuti e restituire i record creati negli ultimi trenta minuti dall'ora corrente.  Impostazione del periodo di tempo a una finestra più ampia di account per il potenziale di latenza dei dati e garantisce che la query restituisce dati per evitare un falso negativo dove, l'avviso non viene mai attivato.  
6. Fare clic su **** per completare la regola di avviso.<br><br> ![Configurare il segnale di avviso](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. Ora spostare nel secondo passaggio, fornire un nome dell'avviso nel **nome regola di avviso** campo, ad esempio **avviso su tutti gli eventi di errore**.  Specificare una **Description** che riporta in dettaglio le specifiche per l'avviso e selezionare **critico (gravità 0)** per il **gravità** valore dalle opzioni fornite.
8. Per attivare immediatamente la regola di avviso durante la creazione, accettare il valore predefinito per **Abilita regola alla creazione**.
9. Per il terzo e ultimo passaggio, si specifica un **gruppo di azioni**, che assicura che vengono eseguite le stesse azioni ogni volta che un avviso viene attivato e può essere usato per ogni regola è definire. Configurare un nuovo gruppo di azioni con le informazioni seguenti:  
   a. Selezionare **nuovo gruppo di azioni** e il **Aggiungi gruppo di azione** viene visualizzato il riquadro.  
   b. Per la **nome gruppo di azione**, specificare un nome, ad esempio **operazioni IT - notificare** e un **nome breve** , ad esempio **itops-n**.  
   c. Verificare i valori predefiniti per **abbonamento** e **gruppo di risorse** siano corretti. In caso contrario, selezionare quella corretta dall'elenco a discesa.   
   d. Nella sezione azioni, specificare un nome per l'azione, ad esempio **Invia messaggio di posta elettronica** e in **tipo di azione** seleziona **posta elettronica/SMS/Push/voce** nell'elenco a discesa. Il **messaggio di posta elettronica/SMS/Push/voce** verrà aperto il riquadro di proprietà a destra per fornire informazioni aggiuntive.  
   e. Nel **messaggio di posta elettronica/SMS/Push/voce** riquadro, selezionare e configurare le preferenze. Ad esempio, attivare **messaggio di posta elettronica** e fornire un indirizzo SMTP di recapitare il messaggio di posta elettronica valido.  
   f. Scegliere **OK** per salvare le modifiche.<br><br> 

    ![Crea nuovo gruppo di azioni](media/configure-azure-monitor/action-group-properties-01.png)

10. Fare clic su **OK** per completare il gruppo di azione. 
11. Fare clic su **Crea regola di avviso** per completare la regola di avviso. L'esecuzione inizia immediatamente.<br><br> ![Completare la creazione nuova regola di avviso](media/configure-azure-monitor/alert-rule-01.png)<br> 

### <a name="test-alerts"></a>Avvisi di test

Per riferimento, si tratta di come appare un avviso di esempio:

![Esempio di avviso tramite posta elettronica](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>Vedere anche

- [Panoramica di spazi diretti di archiviazione](storage-spaces-direct-overview.md)
- Per informazioni più dettagliate, leggere il [documentazione di monitoraggio di Azure](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/tutorial-viewdata).
