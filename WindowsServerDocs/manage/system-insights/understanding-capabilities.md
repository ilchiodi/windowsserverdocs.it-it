---
title: Descrizione delle funzionalità
description: In questo argomento viene definito il concetto di funzionalità nel sistema Insights e introduce le funzionalità predefinite disponibili in Windows Server 2019.
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
ms.openlocfilehash: 21932a3e45d7fc6711400eb30c63c3412bbc116e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830972"
---
# <a name="understanding-capabilities"></a>Descrizione delle funzionalità

>Si applica a: Windows Server 2019

In questo argomento viene definito il concetto di funzionalità nel sistema Insights e introduce le funzionalità predefinite disponibili in Windows Server 2019. 

Questo argomento descrive anche le origini dati, stima le sequenze temporali e gli stati di stima utilizzati per le funzionalità predefinite. 

## <a name="capability-overview"></a>Panoramica delle funzionalità
Una funzionalità di sistema Insights è un Azure machine learning o se il modello delle statistiche che analizza i dati di sistema per offrirti aumentato approfondite il funzionamento della distribuzione. Sistema Insights introduce un set iniziale di funzionalità predefinite e consente di aggiungere nuove funzionalità in modo dinamico, senza la necessità di aggiornare il sistema operativo. 

>[!NOTE]
>Documentazione dettagliata che spiega come creare, aggiungere e le funzionalità di aggiornamento è disponibile [in questo caso](adding-and-developing-capabilities.md), e [documento funzionalità Gestione](managing-capabilities.md) fornisce informazioni più dettagliate su questo funzionalità.

Inoltre, ogni funzionalità viene eseguito in locale in un'istanza di Windows Server e ogni funzionalità può essere gestito individualmente.

### <a name="capability-outputs"></a>Output di funzionalità
Quando una funzionalità viene richiamata, fornisce un output per spiegare il risultato della relativa analisi o la stima. Ogni output può contenere una **lo stato** e una **descrizione dello stato** per descrivere la stima e ogni risultato può contenere specifiche funzionalità dati associati alla stima. Il **descrizione dello stato** aiuta fornisce una spiegazione contestuale per il **stato**, e la funzionalità report di un **OK**, **avviso**, oppure **critico** dello stato. Inoltre, è possibile utilizzare una funzionalità di un' **errore** oppure **None** stato se è stata effettuata alcuna stima. Insieme, di seguito sono gli stati di funzionalità e i relativi significati base: 

- **OK** -tutto sembra corretto.
- **Avviso** : è non richiesto alcun intervento immediato, ma è consigliabile dare un'occhiata. 
- **Critico** -è consigliabile dare un'occhiata a breve. 
- **Errore** -un problema sconosciuto ha causato la possibilità di esito negativo. 
- **Nessuno** -è stata effettuata alcuna stima. Probabilmente a causa di mancanza di dati o qualsiasi altro motivo per non eseguire una stima funzionalità specifiche. 

Inoltre, verranno inseriti i dati di funzionalità specifiche contenuti nel risultato in un file JSON accessibili all'utente e il percorso del file [può essere recuperata tramite PowerShell](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results). 

## <a name="default-capabilities"></a>Funzionalità predefinite
In Windows Server 2019, sistema Insights introduce quattro funzionalità predefinite incentrata sulla previsione della capacità:

- **La previsione della capacità della CPU** -utilizzo CPU di previsioni. 
- **La previsione della capacità di rete** -previsioni di utilizzo per ogni scheda di rete della rete. 
- **Previsione del consumo di spazio di archiviazione totale** -previsioni di consumo di archiviazione totale in tutte le unità locali. 
- **Previsione del consumo di volume** -prevedere il consumo di archiviazione per ogni volume.

Ogni funzionalità consente di analizzare dati cronologici per prevedere l'utilizzo futuro, precedenti e **tutte le funzionalità di previsione sono progettate per prevedere tendenze a lungo termine piuttosto che a breve termine comportamento**, aiutando gli amministratori in modo corretto effettuare il provisioning di hardware e ottimizzare i carichi di lavoro per evitare conflitti di risorse future. Poiché queste funzionalità di concentrarsi sull'utilizzo a lungo termine, queste funzionalità di analizzano i dati giornaliero. 

### <a name="forecasting-model"></a>Modello di previsione
Le funzionalità predefinite usano un modello di previsione per stimare l'utilizzo futuro e per ogni stima, il training del modello in locale sui dati del computer. Questo modello è progettato per aiutare a rilevare le tendenze di più lungo termine e ripetizione del training su ogni istanza del Server di Windows consente la capacità di adattare il comportamento specifico e varie sfumature dell'utilizzo di ogni macchina.

>[!NOTE]
>Determinazione del tipo di modello da usare necessari molti modelli usando un set di dati contenenti decine di migliaia di macchine virtuali di test. Dopo l'analisi e alla regolazione questi modelli, abbiamo deciso di utilizzare un modello di previsione Autoregressivo, perché viene prodotto stime molto accurate e visivamente intuitive non richiedendo troppo tempo per il training di tempo. Questo modello, tuttavia, richiede tre settimane di dati di training, in modo che ogni funzionalità viene utilizzata una tendenza lineare di base fino a quando non sono disponibili tre settimane di dati.

### <a name="forecasting-timelines"></a>Le sequenze temporali di previsione
Le funzionalità predefinite di previsione un determinato numero di giorni nel futuro in base al numero di giorni per cui sono stati raccolti dati. La tabella seguente illustra i tempi di stima di queste funzionalità:

| Dimensioni dei dati di input | Lunghezza previsione |
| --------------- | --------------- |
| 0-5 giorni | Non viene eseguita alcuna stima. |
| 6-180 giorni | 1 o 3 * alle dimensioni dei dati di input |
| 180-365 giorni | 60 giorni | 

### <a name="forecasting-data"></a>Dati della previsione
Ogni funzionalità Analizza i dati giornalieri per prevedere l'utilizzo futuro. Utilizzo della CPU, rete e persino archiviazione, tuttavia, spesso modificabili nel corso della giornata, la regolazione dinamica per i carichi di lavoro nel computer. L'utilizzo non è costante durante tutto il giorno, è importante rappresentare in modo corretto i dati di utilizzo giornalieri in un singolo punto dati. Nella tabella seguente illustra nel dettaglio i punti dati specifici e la modalità di elaborazione dei dati:


| Nome della funzionalità | Origini dati | Logica di filtro |
| --------------- | -------------- | ---------------- |
 Previsione del consumo di volume          | Dimensioni del volume                    | Utilizzo giornaliero massimo              
 Previsione del consumo di spazio di archiviazione totale   | Somma delle dimensioni dei volumi, somma delle dimensioni dei dischi              | Utilizzo giornaliero massimo             
 La previsione della capacità della CPU                | % Tempo processore  | Massima media di 2 ore al giorno   
 La previsione della capacità di rete         | Totale byte/sec         | Massima media di 2 ore al giorno  

Quando si valuta la logica di filtro sopra, è importante notare che ogni funzionalità tenta di informare gli amministratori quando utilizzo futuro chiaramente supera la capacità disponibile, anche se CPU raggiungere momentaneamente il 100% utilizzo, utilizzo della CPU non abbia causa una contesa di risorsa o un calo delle prestazioni significativo. Per la CPU e rete, quindi, non vi sarà un utilizzo elevato prolungato invece momentanea picchi. Calcolarne la media della CPU e utilizzo nel corso della giornata intera di rete, tuttavia, perderebbero informazioni importanti sull'utilizzo, come alcune ore di utilizzo della CPU o utilizzo di rete elevata chiaramente potrebbero compromettere le prestazioni di carichi di lavoro critici. La media massima di 2 ore durante ogni giorno consente di evitare questi estremi e ancora produce dati significativi per ogni funzionalità da analizzare.

Per volume e l'uso di archiviazione totale, tuttavia, l'utilizzo di archiviazione non può superare la capacità disponibile, neppure momentaneamente, pertanto viene usato il massimo utilizzo giornaliero per queste funzionalità. 

### <a name="forecasting-statuses"></a>Gli stati di previsione
Tutte le funzionalità di sistema Insights devono output lo stato associato a ogni stima. Ogni funzionalità predefinita Usa la logica seguente per definire lo stato di ogni stima:
- **OK**: La previsione non superi la capacità disponibile.
- **Avviso**: La previsione supera la capacità disponibile nei successivi 30 giorni. 
- **Critico**: La previsione supera la capacità disponibile nei 7 giorni successivi. 
- **Errore**: La funzionalità si è verificato un errore imprevisto. 
- **Nessuno**: Non sono presenti dati sufficienti per eseguire una stima. Probabilmente a causa di mancanza di dati o perché nessun dato è stato riscontrato di recente.

>[!NOTE]
>Se le previsioni di una funzionalità in più istanze, ad esempio più volumi o le schede di rete - lo stato riflette lo stato più grave in tutte le istanze. Scorriamo i diversi stati per ogni scheda di rete o volumi sono visibili in Windows Admin Center o all'interno dei dati contenuti nell'output di ogni funzionalità. Per istruzioni su come analizzare l'output JSON della funzionalità predefinite, visitare [questo blog](https://aka.ms/systeminsights-mitigationscripts). 


## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica sistema Insights](overview.md)
- [La funzionalità di gestione](managing-capabilities.md)
- [Aggiunta e lo sviluppo di funzionalità](adding-and-developing-capabilities.md)
- [Sistema Insights domande frequenti](faq.md)
