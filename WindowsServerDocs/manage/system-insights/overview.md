---
title: Panoramica sistema Insights
description: Sistema Insights è una nuova funzionalità di analitica predittiva in Windows Server 2019. Le funzionalità di predittive sistema Insights - ogni sono supportate da un modello di machine learning - analisi in locale i dati di sistema di Windows Server, ad esempio i contatori delle prestazioni ed eventi, offrendo così informazioni approfondite sul funzionamento del server e alla possibilità di ridurre il spese operative associate in modo reattivo alla gestione dei problemi nelle distribuzioni.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: ca471e5e747c7aaa369f5725290e16deab7a6660
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887242"
---
# <a name="system-insights-overview"></a>Panoramica sistema Insights

>Si applica a: Windows Server 2019

Sistema Insights è una nuova funzionalità di analitica predittiva in Windows Server 2019. Le funzionalità di predittive sistema Insights - ogni sono supportate da un modello di machine learning - analisi in locale i dati di sistema di Windows Server, ad esempio i contatori delle prestazioni ed eventi, offrendo così informazioni approfondite sul funzionamento del server e alla possibilità di ridurre il spese operative associate in modo reattivo alla gestione dei problemi nelle distribuzioni. 

In Windows Server 2019, sistema Insights viene fornito con funzionalità predefinite di quattro incentrata sulla capacità di previsione, stima future di risorse di calcolo, rete e archiviazione in base i modelli di utilizzo precedenti. Informazioni di sistema viene inoltre fornito con un [infrastruttura estendibile](adding-and-developing-capabilities.md), in modo da Microsoft e parti 3rd possono aggiungere nuove funzionalità predittive per Insights sistema senza aggiornamento del sistema operativo. 

È possibile gestire sistema Insights tramite un'intuitiva [Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) estensione oppure [direttamente tramite PowerShell](https://aka.ms/SystemInsightsPowerShell), e sistema Insights consente di configurare ogni capacità predittiva separatamente in base alle esigenze della distribuzione. Tutti i risultati di stima vengono pubblicati nel registro eventi, che consente di usare [monitoraggio di Azure](https://azure.microsoft.com/services/monitor/) oppure [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) facilmente aggregare e visualizzare le stime in un gruppo di computer.

![Estensione Insights del sistema in Windows Admin Center, che mostra la capacità della CPU previsioni funzionalità con un grafico a cui tracciare la previsione](media/cpu-forecast-2.png)

## <a name="local-functionality"></a>Funzionalità locali
Informazioni di sistema viene eseguito completamente in locale in Windows Server. Utilizzando le nuove funzionalità introdotte in Windows Server 2019, tutti i dati è raccolti, persistente e analizzati direttamente nel computer, consentendo di sfruttare le funzionalità di analitica predittiva senza connettività di cloud.

I dati di sistema sono archiviati nel computer e i dati viene analizzati dalla funzionalità predittive che non richiedono di ripetizione del training nel cloud. Con informazioni dettagliate di sistema, è possibile conservare i dati nel computer e trarre comunque vantaggio dalle funzionalità di analitica predittiva. 

## <a name="get-started"></a>Informazioni di base

<iframe src="https://www.youtube-nocookie.com/embed/AJxQkx5WSaA" width="560" height="315" allowfullscreen></iframe>

>[!TIP]
>Guarda questi brevi video per conoscere le informazioni necessarie per iniziare a usare e gestire in tutta sicurezza sistema Insights: [Introduzione a System Insights in 10 minuti](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

### <a name="requirements"></a>Requisiti
Sistema Insights è disponibile in qualsiasi istanza di Windows Server 2019. Viene eseguito su computer host e guest, in qualsiasi hypervisor e in qualsiasi cloud.

### <a name="install-system-insights"></a>Installa sistema Insights
>[!IMPORTANT]
>Sistema Insights raccoglie e archivia fino a dell'anno dei dati in locale. Se si desidera conservare i dati durante l'aggiornamento del sistema operativo, **assicurarsi di usare In-Place Upgrade**.

#### <a name="install-the-feature"></a>Installare la funzionalità
È possibile installare System Insights usando l'estensione all'interno di Windows Admin Center:

![Esperienza di 0 giorni per estensione Application Insights di sistema.](media/day-0-2.png)

È anche possibile installare System Insights direttamente tramite Server Manager, aggiungere il **System-Insights** funzionalità, oppure tramite PowerShell:

```PowerShell
Add-WindowsFeature System-Insights -IncludeManagementTools
```

## <a name="provide-feedback"></a>Fornire feedback
Saremmo lieti di ricevere commenti e suggerimenti per contribuire a migliorare questa funzionalità. Per inviare commenti e suggerimenti, è possibile usare i canali seguenti:
- **Hub di feedback**: Usare lo strumento di Hub di Feedback in Windows 10 in un file di un bug o commenti e suggerimenti. In tal caso, specificare:
    - **Categoria**: Server 
    - **Sottocategoria**: Informazioni dettagliate di sistema
- **UserVoice**: Inviare richieste di funzionalità tramite nostri [pagina UserVoice](https://windowsserver.uservoice.com/forums/295071-management-tools). Condividere con i colleghi a votare a favore elementi importanti.
- **Email**: Se si vuole inviare commenti e suggerimenti in modo privato al team della funzionalità, inviare un messaggio di posta elettronica system-insights-feed@microsoft.com. Tenere presente che ancora potremmo richiederti di è possibile usare Hub di Feedback o UserVoice.

## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Funzionalità di comprensione](understanding-capabilities.md)
- [La funzionalità di gestione](managing-capabilities.md)
- [Aggiunta e lo sviluppo di funzionalità](adding-and-developing-capabilities.md)
- [Sistema Insights domande frequenti](faq.md)