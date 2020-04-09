---
title: Panoramica di informazioni dettagliate di sistema
description: System Insights è una nuova funzionalità di analisi predittiva di Windows Server 2019. Le funzionalità predittive di System Insights, ognuna supportata da un modello di apprendimento automatico, analizzano localmente i dati di sistema di Windows Server, ad esempio i contatori delle prestazioni e gli eventi, che forniscono informazioni sul funzionamento dei server e consentono di ridurre i costi operativi associati alla gestione riattiva dei problemi nelle distribuzioni.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: e2530ff9ecb4bcf69f2f9a3a452f51696b4466d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815944"
---
# <a name="system-insights-overview"></a>Panoramica di informazioni dettagliate di sistema

>Si applica a: Windows Server 2019

System Insights è una nuova funzionalità di analisi predittiva di Windows Server 2019. Le funzionalità predittive di System Insights, ognuna supportata da un modello di apprendimento automatico, analizzano localmente i dati di sistema di Windows Server, ad esempio i contatori delle prestazioni e gli eventi, che forniscono informazioni sul funzionamento dei server e consentono di ridurre i costi operativi associati alla gestione riattiva dei problemi nelle distribuzioni. 

In Windows Server 2019, System Insights è dotato di quattro funzionalità predefinite incentrate sulla previsione della capacità, sulla previsione di risorse future per il calcolo, la rete e l'archiviazione in base ai modelli di utilizzo precedenti. System Insights viene fornito anche con un' [infrastruttura estensibile](adding-and-developing-capabilities.md), pertanto Microsoft e terze parti possono aggiungere nuove funzionalità predittive a System Insights senza aggiornare il sistema operativo. 

È possibile gestire System Insights tramite un'intuitiva estensione del [centro di amministrazione di Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) o [direttamente tramite PowerShell](https://aka.ms/SystemInsightsPowerShell)e System Insights consente di configurare ogni funzionalità predittiva separatamente in base alle esigenze della distribuzione. Tutti i risultati della stima vengono pubblicati nel registro eventi, che consente di usare [monitoraggio di Azure](https://azure.microsoft.com/services/monitor/) o [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) per aggregare e visualizzare facilmente le stime in un gruppo di computer.

![Estensione di System Insights nell'interfaccia di amministrazione di Windows, che mostra la capacità di previsione della capacità della CPU con un grafico che traccia la previsione](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>Funzionalità locali
System Insights viene eseguito completamente localmente in Windows Server. Grazie alle nuove funzionalità introdotte in Windows Server 2019, tutti i dati vengono raccolti, salvati in modo permanente e analizzati direttamente nel computer, consentendo di realizzare funzionalità di analisi predittiva senza connettività cloud.

I dati di sistema vengono archiviati nel computer e questi dati vengono analizzati da funzionalità predittive che non richiedono la ripetizione del training nel cloud. Con System Insights, è possibile conservare i dati nel computer e sfruttare al tempo stesso le funzionalità di analisi predittiva. 

## <a name="get-started"></a>Attività iniziali

<iframe src=https://www.youtube-nocookie.com/embed/AJxQkx5WSaA width=560 height=315 allowfullscreen></iframe>

>[!TIP]
>Guarda questi brevi video per scoprire le informazioni che ti servono per iniziare e gestire in modo sicuro System Insights: [Introduzione a System Insights in 10 minuti](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

### <a name="requirements"></a>Requisiti
System Insights è disponibile in qualsiasi istanza di Windows Server 2019. Viene eseguito sia su computer host che Guest, su qualsiasi hypervisor e in qualsiasi cloud.

### <a name="install-system-insights"></a>Installare System Insights
>[!IMPORTANT]
>System Insights raccoglie e archivia i dati per un anno in locale. Se si vuole mantenere i dati durante l'aggiornamento del sistema operativo, **assicurarsi di usare l'aggiornamento sul posto**.

#### <a name="install-the-feature"></a>Installare la funzionalità
È possibile installare System Insights usando l'estensione all'interno dell'interfaccia di amministrazione di Windows:

![Esperienza del giorno 0 per l'estensione System Insights.](media/day-0-2.png)

È anche possibile installare System Insights direttamente tramite Server Manager aggiungendo la funzionalità **System-Insights** oppure usando PowerShell:

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>Fornire feedback
Saremmo lieti di ricevere commenti e suggerimenti per contribuire a migliorare questa funzionalità. Per inviare commenti e suggerimenti, è possibile usare i canali seguenti:
- **Hub feedback**: usare lo strumento Hub di feedback in Windows 10 per segnalare un bug o un feedback. Quando si esegue questa operazione, specificare:
    - **Categoria**: Server 
    - **Sottocategoria**: System Insights
- **UserVoice**: inviare le richieste di funzionalità tramite la [pagina UserVoice](https://windowsserver.uservoice.com/forums/295071-management-tools). Condividere con i colleghi gli elementi più importanti per l'utente.
- **Posta elettronica**: se si desidera inviare i propri commenti privati al team di funzionalità, inviare un messaggio di posta elettronica a system-insights-feed@microsoft.com. Si tenga presente che è ancora possibile richiedere l'uso di feedback Hub o UserVoice.

## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Descrizione delle funzionalità](understanding-capabilities.md)
- [Gestione delle funzionalità](managing-capabilities.md)
- [Aggiunta e sviluppo di funzionalità](adding-and-developing-capabilities.md)
- [Domande frequenti su System Insights](faq.md)