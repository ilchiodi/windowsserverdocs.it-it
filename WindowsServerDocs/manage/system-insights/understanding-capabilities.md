---
title: Descrizione delle funzionalità
description: In questo argomento viene definito il concetto di funzionalità di System Insights e vengono introdotte le funzionalità predefinite disponibili in Windows Server 2019.
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
ms.date: 6/05/2018
ms.openlocfilehash: 131fbacaab97c1c2c42920a518ce96ba1b8f5d2b
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465565"
---
# <a name="understanding-capabilities"></a>Descrizione delle funzionalità

>Si applica a: Windows Server 2019

In questo argomento viene definito il concetto di funzionalità di System Insights e vengono introdotte le funzionalità predefinite disponibili in Windows Server 2019. 

In questo argomento vengono inoltre descritte le origini dati, le sequenze temporali di stima e gli Stati di stima utilizzati per le funzionalità predefinite. 

## <a name="capability-overview"></a>Panoramica delle funzionalità
Una funzionalità di System Insights è un modello di apprendimento automatico o di statistiche che analizza i dati di sistema per consentire una maggiore comprensione del funzionamento della distribuzione. System Insights introduce un insieme iniziale di funzionalità predefinite e consente di aggiungere nuove funzionalità in modo dinamico, senza dover aggiornare il sistema operativo. 

>[!NOTE]
>La documentazione dettagliata che illustra come creare, aggiungere e aggiornare le funzionalità è disponibile [qui](adding-and-developing-capabilities.md), mentre [il documento sulle funzionalità di gestione](managing-capabilities.md) fornisce informazioni più dettagliate su questa funzionalità.

Inoltre, ogni funzionalità viene eseguita localmente in un'istanza di Windows Server e ogni funzionalità può essere gestita singolarmente.

### <a name="capability-outputs"></a>Output funzionalità
Quando viene richiamata una funzionalità, fornisce un output per spiegare il risultato dell'analisi o della stima. Ogni output deve contenere uno **stato** e una **Descrizione dello stato** per descrivere la stima e ogni risultato può facoltativamente contenere dati specifici della funzionalità associati alla stima. La **Descrizione dello stato** consente di fornire una spiegazione contestuale dello **stato**e la funzionalità segnala uno stato **OK**, **avviso**o **critico** . Inoltre, una funzionalità può utilizzare un **errore** o **nessuno** stato se non è stata eseguita alcuna stima. Insieme, ecco gli Stati delle funzionalità e i relativi significati di base: 

- **OK** : tutto sembra valido.
- **Avviso** : non è necessario prestare attenzione immediata, ma è opportuno dare un'occhiata. 
- **Critico** : è necessario dare un'occhiata a breve. 
- **Errore** : si è verificato un problema sconosciuto che ha causato l'esito negativo della funzionalità. 
- **None** : non è stata eseguita alcuna stima. Ciò può essere dovuto a una mancanza di dati o a qualsiasi altro motivo specifico della funzionalità per non eseguire una stima. 

Inoltre, tutti i dati specifici della funzionalità contenuti nel risultato verranno inseriti in un file JSON accessibile dall'utente e il percorso del file [può essere trovato usando PowerShell](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results). 

## <a name="default-capabilities"></a>Funzionalità predefinite
In Windows Server 2019, System Insights introduce quattro funzionalità predefinite incentrate sulla previsione della capacità:

- **Previsione della capacità della CPU** : previsioni di utilizzo della CPU. 
- **Previsione della capacità** di rete: prevede l'utilizzo della rete per ogni scheda di rete. 
- **Previsione totale del consumo di spazio di archiviazione** : stima il consumo totale di spazio di archiviazione in tutte le unità locali. 
- **Previsioni sull'utilizzo del volume** : prevede il consumo di spazio di archiviazione per ogni volume.

Ogni funzionalità analizza i dati cronologici passati per stimare l'utilizzo futuro e **tutte le funzionalità di previsione sono progettate per prevedere le tendenze a lungo termine anziché il comportamento a breve termine**, aiutando gli amministratori a eseguire correttamente il provisioning dell'hardware e ottimizzare i carichi di lavoro per evitare future contese di risorse. Poiché queste funzionalità si concentrano sull'utilizzo a lungo termine, queste funzionalità analizzano i dati giornalieri. 

### <a name="forecasting-model"></a>Modello di previsione
Le funzionalità predefinite utilizzano un modello di previsione per stimare l'utilizzo futuro e, per ogni stima, il training del modello viene eseguito localmente sui dati del computer. Questo modello è progettato per consentire di rilevare tendenze a lungo termine e la ripetizione del training in ogni istanza di Windows Server consente di adattare il comportamento specifico e le sfumature dell'utilizzo di ogni computer.

>[!NOTE]
>Determinazione del tipo di modello da usare è necessario testare molti modelli usando un set di dati contenente decine di migliaia di computer. Dopo l'analisi e la modifica di questi modelli, abbiamo deciso di usare un modello di previsione di regressione automatica, in quanto produce stime estremamente accurate e intuitive, senza richiedere troppo tempo per il training. Questo modello, tuttavia, richiede tre settimane di dati di training, quindi ogni funzionalità utilizza una tendenza lineare di base fino a quando non sono disponibili tre settimane di dati.

### <a name="forecasting-timelines"></a>Previsioni temporali
Le funzionalità predefinite prevedono un determinato numero di giorni in futuro in base al numero di giorni per cui sono stati raccolti i dati. Nella tabella seguente vengono illustrate le sequenze temporali di stima di queste funzionalità:

| Dimensioni dei dati di input | Lunghezza previsione |
| --------------- | --------------- |
| 0-5 giorni | Non viene eseguita alcuna stima. |
| 6-180 giorni | 1/3 * dimensioni dei dati di input |
| 180-365 giorni | 60 giorni | 

### <a name="forecasting-data"></a>Previsione dei dati
Ogni funzionalità analizza i dati giornalieri per prevedere un utilizzo futuro. La CPU, la rete e persino l'utilizzo dello spazio di archiviazione, tuttavia, possono variare spesso durante il giorno, adattando in modo dinamico i carichi di lavoro nel computer. Poiché l'utilizzo non è costante durante tutto il giorno, è importante rappresentare correttamente l'utilizzo giornaliero in un singolo punto dati. La tabella seguente illustra in dettaglio i punti dati specifici e il modo in cui vengono elaborati i dati:


| Nome della funzionalità | Origine/i dati | Logica di filtro |
| --------------- | -------------- | ---------------- |
 Previsione dell'utilizzo del volume          | Dimensioni del volume                    | Utilizzo giornaliero massimo              
 Previsione del consumo di spazio di archiviazione totale   | Somma delle dimensioni del volume, somma delle dimensioni del disco              | Utilizzo giornaliero massimo             
 Previsione della capacità della CPU                | % di tempo del processore  | Media massima di 2 ore al giorno   
 Previsione della capacità di rete         | Totale byte/sec         | Media massima di 2 ore al giorno  

Quando si valuta la logica di filtro precedente, è importante tenere presente che ogni funzionalità Cerca di informare gli amministratori quando l'utilizzo futuro supererà significativamente la capacità disponibile, anche se la CPU ha raggiunto il 100% di utilizzo, l'utilizzo della CPU potrebbe non avere ha causato un calo significativo delle prestazioni o una contesa di risorse. Per la CPU e la rete, è necessario disporre di un utilizzo elevato prolungato anziché picchi momentanei. Una media di utilizzo della CPU e della rete nell'intero giorno, tuttavia, perderebbe le informazioni di utilizzo importanti, perché alcune ore di utilizzo elevato della CPU o della rete potrebbero avere un effetto significativo sulle prestazioni dei carichi di lavoro critici. La media massima di 2 ore durante ogni giorno evita questi estremi e produce comunque dati significativi per ogni funzionalità da analizzare.

Per il volume e l'utilizzo totale dello spazio di archiviazione, tuttavia, l'utilizzo dell'archiviazione non può superare la capacità disponibile, anche momentaneamente, quindi viene usato il massimo utilizzo giornaliero per queste funzionalità. 

### <a name="forecasting-statuses"></a>Stati di previsione
Tutte le funzionalità di System Insights devono restituire uno stato associato a ogni stima. Per ogni funzionalità predefinita viene utilizzata la logica seguente per definire ogni stato di stima:
- **OK**: la previsione non supera la capacità disponibile.
- **Avviso**: la previsione supera la capacità disponibile nei 30 giorni successivi. 
- **Critico**: la previsione supera la capacità disponibile nei prossimi 7 giorni. 
- **Errore**: errore imprevisto durante l'esecuzione della funzionalità. 
- **None**: non sono disponibili dati sufficienti per eseguire una stima. Ciò può essere dovuto a una mancanza di dati o perché non è stato segnalato di recente alcun dato.

>[!NOTE]
>Se una funzionalità prevede più istanze, ad esempio più volumi o schede di rete, lo stato riflette lo stato più grave in tutte le istanze. Gli stati individuali per ogni volume o scheda di rete sono visibili nell'interfaccia di amministrazione di Windows o nei dati contenuti nell'output di ciascuna funzionalità. Per istruzioni su come analizzare l'output JSON delle funzionalità predefinite, visitare [questo Blog](https://aka.ms/systeminsights-mitigationscripts). 


## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica di System Insights](overview.md)
- [Gestione delle funzionalità](managing-capabilities.md)
- [Aggiunta e sviluppo di funzionalità](adding-and-developing-capabilities.md)
- [Domande frequenti su System Insights](faq.md)
