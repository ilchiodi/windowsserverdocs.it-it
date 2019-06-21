---
title: Monitorare i server e configurare gli avvisi di monitoraggio di Azure da Windows Admin Center
description: Windows Admin Center (progetto Honolulu) si integra con monitoraggio di Azure
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 03/24/2019
ms.openlocfilehash: 8f7baba465071cc95ab7492037ff25c5cd58219e
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280440"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>Monitorare i server e configurare gli avvisi di monitoraggio di Azure da Windows Admin Center

[Altre informazioni sull'integrazione di Azure con Windows Admin Center.](../plan/azure-integration-options.md)

[Monitoraggio di Azure](https://docs.microsoft.com/azure/azure-monitor/overview) è una soluzione che raccoglie, analizza e agisce sui dati di telemetria da un'ampia gamma di risorse, tra cui Windows Server e le macchine virtuali, sia in locale e nel cloud. Sebbene monitoraggio di Azure estrae i dati da macchine virtuali di Azure e altre risorse di Azure, questo articolo è incentrato sul funzionamento del monitoraggio di Azure con i server locali e macchine virtuali, in particolare con Windows Admin Center. Se si è interessati a informazioni su come è possibile usare monitoraggio di Azure per ottenere avvisi tramite posta elettronica sul cluster iperconvergente, Scopri [tramite Monitoraggio di Azure per inviare messaggi di posta elettronica per gli errori del servizio integrità](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## <a name="how-does-azure-monitor-work"></a>Come funziona il monitoraggio di Azure?
![img](../media/azure-monitor-diagram.png) vengono raccolti i dati generati dai server Windows locali in un'area di lavoro di Log Analitica in Monitoraggio di Azure. All'interno di un'area di lavoro, è possibile abilitare soluzioni di monitoraggio diverse, ovvero set di logica che forniscono informazioni dettagliate per un determinato scenario. Ad esempio Gestione aggiornamenti di Azure, Centro sicurezza di Azure e monitoraggio di Azure per le macchine virtuali sono tutte le soluzioni di monitoraggio che possono essere abilitate in un'area di lavoro. 

Quando si abilita una soluzione di monitoraggio in un'area di lavoro di Log Analitica, tutti i server di report in tale area di lavoro inizierà a raccogliere i dati pertinenti per tale soluzione, in modo che la soluzione può generare informazioni dettagliate per tutti i server dell'area di lavoro. 

Per raccogliere i dati di telemetria in un server locale ed eseguirne il push nell'area di lavoro di Analitica di Log, monitoraggio di Azure richiede l'installazione di Microsoft Monitoring Agent o MMA. Alcune soluzioni di monitoraggio richiedono anche un agente secondario. Monitoraggio di Azure per le macchine virtuali, ad esempio, dipende anche da un agente ServiceMap per questa soluzione offre funzionalità aggiuntive. 

Alcune soluzioni, ad esempio Gestione aggiornamenti di Azure, dipendono anche da automazione di Azure, che consente di gestire in modo centralizzato le risorse in Azure e in ambienti non di Azure. Ad esempio Gestione aggiornamenti di Azure Usa automazione di Azure per pianificare e orchestrare l'installazione degli aggiornamenti tra più computer nell'ambiente in uso, in modo centralizzato, dal portale di Azure.


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>La modalità Windows Admin Center consente di usare monitoraggio di Azure?

Dall'interno WAC, è possibile abilitare due soluzioni di monitoraggio:

- [Gestione degli aggiornamenti Azure](azure-update-management.md) (nello strumento di aggiornamenti)
- Monitoraggio di Azure per le macchine virtuali (in impostazioni server), dette anche macchine virtuali insights

È possibile iniziare a usare monitoraggio di Azure da uno di questi strumenti. Se non si è mai usato Azure Monitor prima, WAC verrà automaticamente il provisioning di un'area di lavoro di Log Analitica e account di automazione di Azure, se necessario, installare e configurare Microsoft Monitoring Agent (MMA) nel server di destinazione. Viene quindi installato la soluzione corrispondente nell'area di lavoro. 

Ad esempio, se prima passare allo strumento gli aggiornamenti per configurare Gestione aggiornamenti di Azure, WAC sarà:

1. Installare MMA nel computer
2. Creare l'area di lavoro di Log Analitica e l'account di automazione di Azure (perché in questo caso, è necessario un account di automazione di Azure)
3. Installare la soluzione gestione aggiornamenti nell'area di lavoro appena creato.

Se si desidera aggiungere un'altra soluzione di monitoraggio da interno WAC nello stesso server, WAC installerà semplicemente tale soluzione nell'area di lavoro esistente a cui tale server è connesso. WAC installerà anche eventuali altri agenti necessari.

Se si connette a un server diverso, ma sia già stata impostata un'area di lavoro di Log Analitica (sia tramite WAC o manualmente nel portale di Azure), è anche possibile installare l'agente MMA nel server e connetterlo fino a un'area di lavoro. Quando ci si connette a un server in un'area di lavoro, verrà avviata automaticamente la raccolta dei dati e creazione di report per le soluzioni installate in tale area di lavoro.

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>Il monitoraggio di Azure per macchine virtuali (noto anche come Informazioni rapide di macchine virtuali)
>Si applica a: Anteprima di Windows Admin Center

Quando si configura il monitoraggio di Azure per le macchine virtuali nel server le impostazioni, Windows Admin Center consente il monitoraggio di Azure per la soluzione di macchine virtuali, noto anche come macchina virtuale insights. Questa soluzione consente di monitorare gli eventi e lo stato del server, creare avvisi di posta elettronica, ottenere una visualizzazione consolidata delle prestazioni del server nel tuo ambiente e visualizzare le app, sistemi e servizi connessi a un determinato server.

> [!NOTE]
> Nonostante il nome della macchina virtuale insights funziona per i server fisici e macchine virtuali.

Con monitoraggio di Azure gratuiti 5 GB di dati/mese/cliente limite, è possibile facilmente provare questa procedura per un o due server senza rischiare di vengano applicati addebiti. Leggere a vedere altri vantaggi dell'onboarding di server in Monitoraggio di Azure, ad esempio per ottenere una visualizzazione consolidata delle prestazioni dei sistemi tra i server nell'ambiente in uso.

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**Configurare il server per l'uso con monitoraggio di Azure**

Dalla pagina di panoramica della connessione a un server, fare clic sul pulsante nuova "Manage alerts", o passa a impostazioni del Server > monitoraggio e avvisi. In questa pagina, eseguire l'onboarding al server di monitoraggio di Azure, fare clic su "Configura" e completare il riquadro di configurazione. Interfaccia di amministrazione si occupa del provisioning l'area di lavoro di Azure Log Analitica, di installazione dell'agente necessario e garantire che la soluzione di insights della macchina virtuale è configurata. Al termine dell'operazione, il server invierà i dati dei contatori delle prestazioni per il monitoraggio di Azure, che consente di visualizzare e creare avvisi di posta elettronica basati su questo server, dal portale di Azure.

### <a name="create-email-alerts"></a>**Creare avvisi di posta elettronica**

Una volta dopo la connessione del server di monitoraggio di Azure, è possibile usare i collegamenti ipertestuali intelligenti entro le impostazioni > monitoraggio e avvisi di pagina per passare al portale di Azure. Interfaccia di amministrazione abilita automaticamente i contatori delle prestazioni da raccogliere, in modo da poter facilmente [creare un nuovo avviso](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) personalizzazione di una delle molte query predefinite o scrivendo un proprio.

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>\* * Ottenere una visualizzazione consolidata in più server * *

Se è integrata più server per una singola area di lavoro di Log Analitica all'interno di monitoraggio di Azure, è possibile ottenere una visualizzazione consolidata di tutti i server dal [soluzione Insights macchine virtuali](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) all'interno di monitoraggio di Azure.  Si noti che solo le schede delle prestazioni e le mappe di Insights di macchine virtuali per monitoraggio di Azure funzionerà con i server locali: le funzioni di scheda integrità solo con macchine virtuali di Azure. Per visualizzare tali informazioni nel portale di Azure, passare a monitoraggio di Azure > macchine virtuali (con informazioni rapide) e passare alla scheda "Prestazioni" o "Mappe".

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**Visualizzare le app, sistemi e servizi connessi a un determinato server**

Quando il centro di amministrazione carica un server all'interno della soluzione di insights VM all'interno di monitoraggio di Azure, rende disponibili anche una funzionalità denominata [mapping dei servizi](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Questa funzionalità consente di individuare i componenti dell'applicazione automaticamente e mappa la comunicazione tra servizi in modo che sia possibile visualizzare facilmente le connessioni tra server con ottimo dettaglio dal portale di Azure. È possibile trovarlo, passare al portale di Azure > monitoraggio di Azure > macchine virtuali (con informazioni dettagliate) e passando alla scheda "Mappe".

> [!NOTE]
> Le visualizzazioni per le macchine virtuali Insights per monitoraggio di Azure sono attualmente disponibili in 6 aree pubbliche.  Per informazioni più recenti, visitare il [monitoraggio di Azure per la documentazione di macchine virtuali](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  È necessario distribuire l'area di lavoro di Log Analitica in una delle aree supportate per ottenere i vantaggi aggiuntivi offerti dalla soluzione di macchine virtuali di Insights descritta in precedenza.

## <a name="disabling-monitoring"></a>Disabilitazione del monitoraggio

Per disconnettere completamente i server nell'area di lavoro di Log Analitica, disinstallare l'agente MMA. Ciò significa che questo server non invierà più dati nell'area di lavoro e tutte le soluzioni installate nell'area di lavoro non saranno più raccogliere ed elaborare i dati dal tale server. Tuttavia, questa operazione non influenza l'area di lavoro stessa – tutte le risorse di creazione di report all'area di lavoro continuerà a eseguire questa operazione. Per disinstallare l'agente MMA in WAC, andare a App e funzionalità, trovare Microsoft Monitoring Agent e fare clic su Disinstalla.

Se si desidera disattivare una soluzione specifica all'interno di un'area di lavoro, dovrai [rimuovere la soluzione di monitoraggio dal portale di Azure](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). Rimozione di una soluzione di monitoraggio significa che le informazioni dettagliate create da tale soluzione non è più verranno generate per _qualsiasi_ dei server di report all'area di lavoro. Ad esempio, se Disinstalla il monitoraggio di Azure per la soluzione di macchine virtuali, non è più vedremo informazioni dettagliate sulle prestazioni della macchina virtuale o server da qualsiasi computer connesso a area di lavoro personale.