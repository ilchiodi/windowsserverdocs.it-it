---
title: Origini dati di System Insights
description: Quando si scrive una nuova funzionalità in System Insights, è possibile specificare origini dati nuove o esistenti da raccogliere localmente e analizzare. Questo argomento descrive le origini dati che è possibile scegliere per la registrazione di una nuova funzionalità.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 920c5fa5919fd2c35edb99cc4e724745715091c7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858424"
---
# <a name="system-insights-data-sources"></a>Origini dati di System Insights

>Si applica a: Windows Server 2019

System Insights introduce funzionalità di raccolta dati estendibili. Quando si scrive una nuova funzionalità, è possibile specificare origini dati nuove o esistenti per la raccolta locale e l'analisi. Questo argomento descrive le origini dati che è possibile scegliere per la registrazione di una nuova funzionalità.

## <a name="data-sources"></a>Origini dati
Quando si scrive una nuova funzionalità, è necessario identificare le origini dati specifiche da raccogliere per ogni funzionalità. Le origini dati specificate verranno raccolte e rese disponibili direttamente nel computer ed è possibile scegliere tra tre tipi di origini dati:

- **Contatori delle prestazioni**: 
    - Specificare il percorso del contatore, il nome e le istanze e System Insights raccoglie i dati rilevanti restituiti da questi contatori delle prestazioni. 

- **Eventi di sistema**:
    - Specificare il nome del canale e l'ID evento e System Insights registrerà il numero di volte in cui si è verificato l'evento.

- **Serie Nota**
    - System Insights raccoglie alcune informazioni di base sul computer per alcune risorse ben definite. Queste serie vengono usate per le funzionalità predefinite, ma possono anche essere usate da qualsiasi funzionalità personalizzata. Vengono raccolte le informazioni seguenti:

        - **Disco**: 
            - *Proprietà*: GUID
            - *Dati*: dimensioni
        - **Volume**:
            - *Proprietà*: UniqueId, LetteraUnità, FileSystemLabel, Size
            - *Dati*: dimensioni usate
        - **Scheda di rete**:
            - *Proprietà*: InterfaceGuid, InterfaceDescription, Speed
            - *Dati*: byte ricevuti/sec, byte inviati/sec, totale byte/sec
        - **CPU**: 
            - *Proprietà*:-
            - *Dati*:% tempo processore

    - Specificare una serie nota e System Insights restituirà i dati raccolti da tale serie. 


## <a name="retention-timelines-and-collection-intervals"></a>Tempistiche di conservazione e intervalli di raccolta
Le origini dati sopra elencate hanno tempistiche di conservazione e intervalli di raccolta diversi. La tabella seguente mostra la durata e la frequenza di raccolta di ogni origine dati:

| Origine dati | Sequenza temporale conservazione | Intervallo di raccolta |
| --------------- | --------------- | ----------- |
| Contatori di prestazioni | 3 mesi | 15 minuti |
| Eventi di sistema | 3 mesi | 15 minuti |
| Serie Nota | 1 anno | 1 ora |


### <a name="aggregation-types"></a>Tipi di aggregazione
Poiché ogni serie registra un solo punto dati per ogni intervallo di raccolta, a ogni serie è associato un tipo di aggregazione associata. La tabella seguente descrive l'origine dati e il tipo di aggregazione corrispondente:

>[!NOTE]
>Per i contatori delle prestazioni, è possibile scegliere tra diversi tipi di aggregazione.

| Origine dati | Tipi di aggregazione |
| --------------- | --------------- |
| Contatori di prestazioni | Somma, media, Max, min |
| Eventi di sistema | Conteggio |
| Serie Nota del disco | Ultimo (valore più recente nell'intervallo di raccolta) |
| Serie Note del volume | Ultimo (valore più recente nell'intervallo di raccolta) |
| Serie Nota CPU | Media |
| Serie Nota di rete | Media |

## <a name="data-footprint"></a>Impronta dati

System Insights raccoglie tutti i dati localmente sull'unità C (C:). In generale, il footprint dei dati di System Insights è modesto. Dipende direttamente dal tipo e dal numero di origini dati specificate da ogni funzionalità e la tabella seguente illustra in dettaglio l'utilizzo dell'archiviazione per ogni tipo di dati:

| Origine dati | Impronta massima |
| --------------- | --------------- |
| Contatori di prestazioni | 240 KB |
| Eventi di sistema | 200 KB |
| Serie Nota del disco | 200 KB per disco |
| Serie Note del volume | 300 KB per volume |
| Serie Nota CPU | 100 KB |
| Serie Nota di rete | 300 KB per scheda di rete |

>[!NOTE]
>**Per le funzionalità di previsione predefinite, il footprint massimo deve essere inferiore a 10 MB per la maggior parte dei computer autonomi.** 

## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica di System Insights](overview.md)
- [Descrizione delle funzionalità](understanding-capabilities.md)
- [Gestione delle funzionalità](managing-capabilities.md)
- [Aggiunta e sviluppo di funzionalità](adding-and-developing-capabilities.md)
- [Domande frequenti su System Insights](faq.md)
