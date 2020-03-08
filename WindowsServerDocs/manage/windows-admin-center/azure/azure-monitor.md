---
title: Monitorare i server e configurare gli avvisi con monitoraggio di Azure dall'interfaccia di amministrazione di Windows
description: L'interfaccia di amministrazione di Windows (Project Honolulu) si integra con monitoraggio di Azure
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 03/24/2019
ms.openlocfilehash: 28108a79bbdc654f6437a698c158a3f74d4423ba
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371129"
---
# <a name="monitor-servers-and-configure-alerts-with-azure-monitor-from-windows-admin-center"></a>Monitorare i server e configurare gli avvisi con monitoraggio di Azure dall'interfaccia di amministrazione di Windows

[Altre informazioni sull'integrazione di Azure con l'interfaccia di amministrazione di Windows.](../plan/azure-integration-options.md)

[Monitoraggio di Azure](https://docs.microsoft.com/azure/azure-monitor/overview) è una soluzione che raccoglie, analizza e agisce sui dati di telemetria da un'ampia gamma di risorse, tra cui server e macchine virtuali Windows, sia in locale che nel cloud. Anche se monitoraggio di Azure estrae i dati dalle macchine virtuali di Azure e da altre risorse di Azure, questo articolo è incentrato sul funzionamento di monitoraggio di Azure con server e macchine virtuali locali, in particolare con l'interfaccia di amministrazione di Windows. Per informazioni su come usare monitoraggio di Azure per ricevere avvisi di posta elettronica sul cluster iperconvergente, vedere l'articolo relativo [all'uso di monitoraggio di Azure per inviare messaggi di posta elettronica per gli errori di servizio integrità](https://docs.microsoft.com/windows-server/storage/storage-spaces/configure-azure-monitor).

## <a name="how-does-azure-monitor-work"></a>Funzionamento di monitoraggio di Azure
![IMG](../media/azure-monitor-diagram.png) i dati generati da server Windows locali vengono raccolti in un'area di lavoro Log Analytics in monitoraggio di Azure. All'interno di un'area di lavoro è possibile abilitare varie soluzioni di monitoraggio, ovvero set di logica che forniscono informazioni dettagliate per un particolare scenario. Ad esempio, Gestione aggiornamenti di Azure, il Centro sicurezza di Azure e Monitoraggio di Azure per le macchine virtuali sono tutte soluzioni di monitoraggio che possono essere abilitate all'interno di un'area di lavoro. 

Quando si abilita una soluzione di monitoraggio in un'area di lavoro Log Analytics, tutti i server che inviano report all'area di lavoro inizieranno a raccogliere i dati rilevanti per tale soluzione, in modo che la soluzione possa generare informazioni dettagliate per tutti i server nell'area di lavoro. 

Per raccogliere dati di telemetria in un server locale ed eseguirne il push nell'area di lavoro Log Analytics, monitoraggio di Azure richiede l'installazione del Microsoft Monitoring Agent o MMA. Alcune soluzioni di monitoraggio richiedono anche un agente secondario. Ad esempio, Monitoraggio di Azure per le macchine virtuali dipende anche da un agente ServiceMap per le funzionalità aggiuntive fornite da questa soluzione. 

Alcune soluzioni, come Azure Gestione aggiornamenti, dipendono anche da automazione di Azure, che consente di gestire centralmente le risorse in ambienti Azure e non Azure. Azure Gestione aggiornamenti, ad esempio, USA automazione di Azure per pianificare e orchestrare l'installazione di aggiornamenti tra i computer nell'ambiente, in modo centralizzato, dall'portale di Azure.


## <a name="how-does-windows-admin-center-enable-you-to-use-azure-monitor"></a>In che modo l'interfaccia di amministrazione di Windows consente di usare monitoraggio di Azure?

Dall'interno di WAC è possibile abilitare due soluzioni di monitoraggio:

- [Azure Gestione aggiornamenti](azure-update-management.md) (nello strumento aggiornamenti)
- Monitoraggio di Azure per le macchine virtuali (in Impostazioni Server), a. k. informazioni dettagliate sulle macchine virtuali

È possibile iniziare a usare monitoraggio di Azure da uno di questi strumenti. Se il monitoraggio di Azure non è mai stato usato in precedenza, WAC effettuerà automaticamente il provisioning di un'area di lavoro Log Analytics (e dell'account di automazione di Azure, se necessario) e installerà e configurerà il Microsoft Monitoring Agent (MMA) nel server di destinazione La soluzione corrispondente verrà quindi installata nell'area di lavoro. 

Ad esempio, se si passa prima allo strumento aggiornamenti per configurare Azure Gestione aggiornamenti, WAC sarà:

1. Installare MMA nel computer
2. Creare l'area di lavoro Log Analytics e l'account di automazione di Azure (perché in questo caso è necessario un account di automazione di Azure)
3. Installare la soluzione Gestione aggiornamenti nell'area di lavoro appena creata.

Se si vuole aggiungere un'altra soluzione di monitoraggio da WAC nello stesso server, WAC installa semplicemente tale soluzione nell'area di lavoro esistente a cui è connesso il server. WAC installerà inoltre eventuali altri agenti necessari.

Se ci si connette a un server diverso, ma è già stata configurata un'area di lavoro Log Analytics (tramite WAC o manualmente nel portale di Azure), è anche possibile installare l'agente MMA nel server e connetterlo a un'area di lavoro esistente. Quando si connette un server in un'area di lavoro, viene automaticamente avviata la raccolta dei dati e la creazione di report per le soluzioni installate nell'area di lavoro.

## <a name="azure-monitor-for-virtual-machines-aka-virtual-machine-insights"></a>Monitoraggio di Azure per le macchine virtuali (noto anche come Informazioni dettagliate sulla macchina virtuale
>Si applica a: anteprima di Windows Admin Center

Quando si configura Monitoraggio di Azure per le macchine virtuali in Impostazioni server, l'interfaccia di amministrazione di Windows Abilita la soluzione Monitoraggio di Azure per le macchine virtuali, nota anche come Virtual Machine Insights. Questa soluzione consente di monitorare l'integrità del server e gli eventi, creare avvisi di posta elettronica, ottenere una visualizzazione consolidata delle prestazioni del server nell'ambiente e visualizzare le app, i sistemi e i servizi connessi a un determinato server.

> [!NOTE]
> Nonostante il nome, VM Insights funziona anche per i server fisici e le macchine virtuali.

Con i 5 GB di dati/mese/indennità clienti di monitoraggio di Azure, è possibile provare facilmente a eseguire questa operazione per un server o due senza doversi preoccupare di ricevere addebiti. Continua a leggere per visualizzare i vantaggi aggiuntivi dei server di onboarding in monitoraggio di Azure, ad esempio ottenere una visualizzazione consolidata delle prestazioni dei sistemi tra i server nell'ambiente.

### <a name="set-up-your-server-for-use-with-azure-monitor"></a>**Configurare il server per l'uso con monitoraggio di Azure**

Dalla pagina Panoramica di una connessione al server, fare clic sul pulsante nuovo "Gestisci avvisi" oppure passare a impostazioni server > monitoraggio e avvisi. In questa pagina, caricare il server in monitoraggio di Azure facendo clic su "Configura" e completando il riquadro di installazione. L'interfaccia di amministrazione esegue il provisioning dell'area di lavoro di Azure Log Analytics, installando l'agente necessario e verificando che la soluzione VM Insights sia configurata. Al termine, il server invierà i dati dei contatori delle prestazioni a monitoraggio di Azure, consentendo di visualizzare e creare avvisi di posta elettronica basati su questo server, dal portale di Azure.

### <a name="create-email-alerts"></a>**Creare avvisi di posta elettronica**

Dopo aver collegato il server a monitoraggio di Azure, è possibile usare i collegamenti ipertestuali intelligenti all'interno della pagina Impostazioni > monitoraggio e avvisi per passare al portale di Azure. L'interfaccia di amministrazione consente di raccogliere automaticamente i contatori delle prestazioni, in modo da poter [creare facilmente un nuovo avviso](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) personalizzando una delle numerose query predefinite o scrivendone di personalizzate.

### <a name="get-a-consolidated-view-across-multiple-servers-"></a>\* * Ottenere una visualizzazione consolidata tra più server * *

Se si caricano più server in una singola area di lavoro Log Analytics all'interno di monitoraggio di Azure, è possibile ottenere una visualizzazione consolidata di tutti questi server dalla [soluzione macchine virtuali Insights](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview) in monitoraggio di Azure.  Si noti che solo le schede prestazioni e mappe di macchine virtuali informazioni dettagliate per monitoraggio di Azure funzioneranno con i server locali: la scheda integrità funziona solo con le macchine virtuali di Azure. Per visualizzarlo nella portale di Azure, passare a monitoraggio di Azure > macchine virtuali (in Insights) e passare alle schede "prestazioni" o "mappe".

### <a name="visualize-apps-systems-and-services-connected-to-a-given-server"></a>**Visualizza le app, i sistemi e i servizi connessi a un determinato server**

Quando l'interfaccia di amministrazione carica un server nella soluzione VM Insights all'interno di monitoraggio di Azure, viene anche accesa una funzionalità denominata [mapping dei servizi](https://docs.microsoft.com/azure/azure-monitor/insights/service-map). Questa funzionalità consente di individuare automaticamente i componenti dell'applicazione e di mappare le comunicazioni tra i servizi in modo che sia possibile visualizzare facilmente le connessioni tra i server con informazioni dettagliate dal portale di Azure. Per trovarlo, passare alla portale di Azure > monitoraggio di Azure > macchine virtuali (in Insights) e passare alla scheda "Maps".

> [!NOTE]
> Le visualizzazioni per le informazioni dettagliate sulle macchine virtuali per monitoraggio di Azure sono attualmente disponibili in 6 aree pubbliche.  Per informazioni aggiornate, vedere la [documentazione monitoraggio di Azure per le macchine virtuali](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-onboard#log-analytics).  Per ottenere i vantaggi aggiuntivi offerti dalla soluzione Virtual Machine Insights descritta in precedenza, è necessario distribuire l'area di lavoro Log Analytics in una delle aree supportate.

## <a name="disabling-monitoring"></a>Disabilitazione del monitoraggio

Per disconnettere completamente il server dall'area di lavoro Log Analytics, disinstallare l'agente MMA. Questo significa che il server non invierà più dati all'area di lavoro e tutte le soluzioni installate in tale area di lavoro non raccoglieranno ed elaborano i dati da tale server. Tuttavia, ciò non influisce sull'area di lavoro stessa. tutte le risorse che inviano report all'area di lavoro continueranno a farlo. Per disinstallare l'agente MMA in WAC, passare a app & funzionalità, trovare il Microsoft Monitoring Agent e fare clic su Disinstalla.

Se si desidera disattivare una soluzione specifica in un'area di lavoro, sarà necessario [rimuovere la soluzione di monitoraggio dalla portale di Azure](https://docs.microsoft.com/azure/azure-monitor/insights/solutions#remove-a-management-solution). La rimozione di una soluzione di monitoraggio indica che le informazioni create da tale soluzione non verranno più generate _per i_ server che inviano report all'area di lavoro. Se, ad esempio, si disinstalla la soluzione Monitoraggio di Azure per le macchine virtuali, non vengono più visualizzate informazioni dettagliate sulle prestazioni delle macchine virtuali o dei server da qualsiasi computer connesso all'area di lavoro.