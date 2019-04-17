---
title: Il monitoraggio dei server e configurare gli avvisi con Azure Monitor da Windows Admin Center
description: Windows Admin Center (Project Honolulu) si integra con Monitor di Azure
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 6ada708bf7dd8cd08e1bc2620be5244a07beac7d
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297023"
---
# Il monitoraggio dei server e configurare gli avvisi con Azure Monitor da Windows Admin Center

[Altre informazioni sull'integrazione di Azure con Windows Admin Center.](../plan/azure-integration-options.md)

[Monitor di Azure](https://docs.microsoft.com/azure/azure-monitor/overview) è una soluzione che raccoglie e analizza agisce di telemetria da una vasta gamma di risorse, tra cui Windows Server e macchine virtuali, sia in locale e nel cloud. Anche se Monitor Azure trascina i dati da macchine virtuali di Azure e altre risorse di Azure, questo articolo è incentrato su come monitorare Azure funziona con i server in locale e le macchine virtuali, in particolare con Windows Admin Center. Se sei interessato a informazioni su come utilizzare Azure Monitor per ottenere avvisi tramite posta elettronica su cluster iper-convergenti, leggere [sull'utilizzo di monitoraggio di Azure per inviare messaggi di posta elettronica per gli errori del servizio di integrità](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## Come funziona Monitor Azure?
![img](../media/azure-monitor-diagram.png) generati dal server di Windows in locale di dati raccolti in un'area di lavoro di Log Analitica in Azure Monitor. All'interno di un'area di lavoro, è possibile abilitare diverse soluzioni di monitoraggio, ovvero imposta della logica che forniscono informazioni approfondite per un determinato scenario. Ad esempio, gestione degli aggiornamenti di Azure, Centro sicurezza di Azure e Monitor di Azure per le macchine virtuali sono tutte le soluzioni di monitoraggio che possono essere abilitate all'interno di un'area di lavoro. 

Quando si abilita una soluzione di monitoraggio in un'area di lavoro di Log Analitica, tutti i server invia segnalazioni a tale area di lavoro verrà iniziare a raccogliere i dati rilevanti per che tale soluzione, in modo che la soluzione generare informazioni approfondite per tutti i server nell'area di lavoro. 

Per raccogliere i dati di telemetria su un server locale e si push all'area di lavoro Analitica Log, monitorare Azure richiede l'installazione di Microsoft Monitoring Agent o il MMA. Alcune soluzioni di monitoraggio richiedono anche un agente secondario. Monitor di Azure per le macchine virtuali, ad esempio, dipende anche da un agente ServiceMap per funzionalità aggiuntive che fornisce questa soluzione. 

Alcune soluzioni, ad esempio la gestione degli aggiornamenti Azure, anche dipendono dall'automazione di Azure, che consente di gestire centralmente le risorse in ambienti non Azure e Azure. Ad esempio, gestione degli aggiornamenti Azure utilizza l'automazione di Azure per pianificare e gestire centralmente, installazione degli aggiornamenti in computer nel tuo ambiente, dal portale di Azure.


## Come Windows Admin Center consentono di usare Azure Monitor?

All'interno di WAC, è possibile abilitare soluzioni di monitoraggio di due:

- [Gestione degli aggiornamenti di Azure](azure-update-management.md) (nello strumento aggiornamenti)
- Monitoraggio di Azure per le macchine virtuali (in server impostazioni), detta anche informazioni approfondite di macchine virtuali

È possibile iniziare a usare Azure Monitor uno di questi strumenti. Se non hai mai usato Azure Monitor prima, WAC automaticamente il provisioning per un'area di lavoro di Log Analitica (e account Azure automazione, se necessario) e installare e configurare Microsoft Monitoring Agent (MMA) nel server di destinazione. La soluzione corrispondente verrà quindi installato nell'area di lavoro. 

Ad esempio, se prima di tutto Vai allo strumento di aggiornamenti per la gestione degli aggiornamenti Azure di installazione, verrà WAC:

1. Installare il MMA nel computer
2. Creare l'area di lavoro di Log Analitica e l'account Azure automazione (dal momento che in questo caso, è necessario un account Azure automazione)
3. Installa la soluzione di gestione degli aggiornamenti nell'area di lavoro appena creato.

Se vuoi aggiungere un'altra soluzione di monitoraggio all'interno di WAC nello stesso server, WAC installerà semplicemente tale soluzione nell'area di lavoro esistente a cui è connesso tale server. WAC installerà inoltre eventuali altri agenti necessari.

Se la connessione a un altro server, ma dispone già di installazione di un'area di lavoro di Log Analitica (o tramite WAC manualmente nel portale di Azure), puoi anche installare l'agente MMA nel server e connetterlo fino a un'area di lavoro esistente. Quando ti Connetti a un server in un'area di lavoro, verrà avviata automaticamente la raccolta dei dati e la segnalazione alle soluzioni installate nell'area di lavoro.

## Azure monitorare per macchine virtuali (ovvero Informazioni dettagliate di macchina virtuale)
>Si applica a: Windows Admin Center Preview

Quando configuri Azure Monitor per le macchine virtuali nel server le impostazioni, Windows Admin Center consente il monitoraggio di Azure per la soluzione di macchine virtuali, noto anche come informazioni approfondite macchina virtuale. Questa soluzione consente di monitorare gli eventi e l'integrità di server, creare avvisi tramite posta elettronica, ottenere una panoramica consolidata delle prestazioni del server attraverso l'ambiente e ti consente di visualizzare le app, sistemi e servizi connessi a un determinato server.

> [!NOTE]
> Nonostante il nome, informazioni approfondite della macchina virtuale funziona per server fisici, nonché le macchine virtuali.

Con Azure Monitor gratuiti 5 GB di dati/mese/cliente consentita, si possa facilmente proveremo questo per un server o due senza preoccuparsi di recupero di addebito. Leggere a vedere altri vantaggi offerti da onboarding di server in Azure Monitor, ad esempio come ottenere una visualizzazione consolidata delle prestazioni dei sistemi tra i server nel tuo ambiente.

### **Configurare il server per l'uso con Monitor di Azure**

Nella pagina della panoramica di una connessione al server, fare clic sul pulsante "Gestisci avvisi" nuovo o Vai a impostazioni del Server gt _ monitoraggio e avvisi. All'interno di questa pagina, onboarding il server Azure monitor facendo clic su "Configura" e il completamento del riquadro di installazione. Interfaccia di amministrazione si occupa di provisioning l'area di lavoro di Azure Log Analitica, l'installazione dell'agente necessaria e garantire la soluzione di informazioni approfondite della macchina virtuale è configurato. Al termine, il server invia i dati dei contatori delle prestazioni a Monitor di Azure, offrendo la possibilità di visualizzare e creare avvisi tramite posta elettronica basati su questo server, dal portale di Azure.

### **Creare avvisi tramite posta elettronica**

Dopo aver collegato il server Azure monitor, è possibile utilizzare i collegamenti ipertestuali intelligenti all'interno della pagina Impostazioni gt _ monitoraggio e avvisi per passare al portale di Azure. Interfaccia di amministrazione abilita automaticamente i contatori delle prestazioni essere raccolti, pertanto è possibile [creare un nuovo avviso](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) personalizzazione una delle molte query predefinite, o scrivendo il tuo.

### * * Ottenere una panoramica consolidata in più server * *

Se si onboarding più server per un singolo Log Analitica nell'area di lavoro all'interno di monitoraggio di Azure, è possibile ottenere una visualizzazione di tutti questi server consolidata dalla [soluzione informazioni approfondite macchine virtuali](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) all'interno di monitoraggio di Azure.  Si noti che solo le schede delle prestazioni e le mappe di informazioni dettagliate di macchine virtuali di Azure Monitor funzioni con il server locale: le funzioni di scheda sull'integrità solo con macchine virtuali di Azure. Per visualizzare questo nel portale di Azure, Vai al Monitor Azure gt _ le macchine virtuali (con informazioni approfondite) e passa alla scheda "Prestazioni" o "Mapping".

### **Ti consente di visualizzare le app, sistemi e servizi connessi a un determinato server**

Quando amministrazione onboards un server nella soluzione di informazioni approfondite della macchina virtuale all'interno di monitoraggio di Azure, anche illumina una funzionalità denominata [Mappa di servizio](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Questa funzionalità è automaticamente individua i componenti dell'applicazione e associa la comunicazione tra i servizi in modo che è possibile visualizzare facilmente le connessioni tra i server con dettaglio dal portale di Azure. Puoi trovare questo passando a di Azure nel portale gt _ gt _ Azure monitorare le macchine virtuali (con informazioni approfondite) e lo spostamento alla scheda "Mapping".

> [!NOTE]
> Gli effetti grafici per informazioni dettagliate sulle macchine virtuali di Azure Monitor sono offerti in aree geografiche pubbliche 6 attualmente.  Per informazioni più recenti, controlla il [Monitoraggio di Azure per la documentazione di macchine virtuali](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Devi distribuire l'area di lavoro di Log Analitica in una delle aree supportate per ottenere vantaggi aggiuntivi forniti dalla soluzione informazioni approfondite macchine virtuali descritta in precedenza.

## La disabilitazione di monitoraggio

Per disconnettere completamente il server dall'area di lavoro di Log Analitica, disinstallare l'agente MMA. Ciò significa che il server non invierà più dati per l'area di lavoro e tutte le soluzioni installate nell'area di lavoro non saranno più raccogliere e trattare i dati da tale server. Tuttavia, questo non incide l'area di lavoro-tutte le risorse segnalazione a tale area di lavoro continuerà a eseguire questa operazione. Per disinstallare l'agente MMA all'interno di WAC, Vai alle funzionalità & le App e Disinstalla trovare Microsoft Monitoring Agent.

Se vuoi disattivare una soluzione specifica all'interno di un'area di lavoro, dovrai [rimuovere la soluzione di monitoraggio dal portale di Azure](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). Rimozione di una soluzione di monitoraggio significa che le informazioni approfondite creati da che non è più soluzione verrà generata per _qualsiasi_ server invia segnalazioni a tale area di lavoro. Ad esempio, se Disinstalla il monitoraggio di Azure per la soluzione di macchine virtuali, non vedranno più informazioni approfondite sulle prestazioni della macchina virtuale o un server da qualsiasi dei computer connessi alla mia area di lavoro.