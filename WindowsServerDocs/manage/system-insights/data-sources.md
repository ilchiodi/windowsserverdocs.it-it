---
title: Origini dati di sistema Insights
description: Quando si scrive una nuova funzionalità nel sistema di Insights, è possibile specificare le origini dati nuovo o esistente per raccogliere in locale e analizzare. In questo argomento descrive le origini dati che è possibile scegliere quando si registra una nuova funzionalità.
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
ms.date: 7/31/2018
ms.openlocfilehash: 9b46db90787d24a173ffa472ec1ecb8eaffe054b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845412"
---
# <a name="system-insights-data-sources"></a>Origini dati di sistema Insights

>Si applica a: Windows Server 2019

Sistema Insights introduce funzionalità di raccolta dati estensibile. Quando si scrive una nuova funzionalità, è possibile specificare le origini dati nuovo o esistente per raccogliere in locale e analizzare. In questo argomento descrive le origini dati che è possibile scegliere quando si registra una nuova funzionalità.

## <a name="data-sources"></a>Origini dati
Quando si scrive una nuova funzionalità, è necessario identificare le origini di dati specifico da raccogliere per ogni funzionalità. Le origini di dati specificato verranno raccolto e rese persistenti direttamente nel computer in uso ed è possibile scegliere tra tre tipi di origini dati:

- **I contatori delle prestazioni**: 
    - Specificare il percorso del contatore, nome e le istanze e sistema Insights raccoglie i dati rilevanti segnalati dai contatori delle prestazioni. 

- **Gli eventi di sistema**:
    - Specificare l'ID evento e nome del canale e sistema Insights registrerà quante volte si è verificato un evento.

- **Serie Well-Known**
    - Sistema Insights raccoglie informazioni di base nel computer per le risorse di alcuni e ben definiti. Queste serie vengono usate per le funzionalità predefinite, ma possono anche essere utilizzati da qualsiasi capacità personalizzato. Questi raccogliere le informazioni seguenti:

        - **Disco**: 
            - *Proprietà*: GUID
            - *Dati*: Dimensione
        - **Volume**:
            - *Proprietà*: UniqueId, DriveLetter, FileSystemLabel, Size
            - *Dati*: Dimensioni di utilizzo
        - **Scheda di rete**:
            - *Proprietà*: Velocità InterfaceGuid, InterfaceDescription,
            - *Dati*: Byte ricevuti/sec, byte inviati/sec, Totale byte/sec
        - **CPU**: 
            - *Proprietà*:-
            - *Dati*: % tempo processore

    - Specificare una serie nota e sistema Insights verranno restituiti i dati raccolti da tale serie. 


## <a name="retention-timelines-and-collection-intervals"></a>Intervalli di raccolta e sequenze temporali di conservazione
Le origini dati precedenti hanno intervalli sequenze temporali e raccolta di conservazione diversi. Nella tabella seguente vengono visualizzati come long e della frequenza in cui verrà raccolti ogni origine dati:

| Origine dati | Sequenza temporale di conservazione | Intervallo di raccolta |
| --------------- | --------------- | ----------- |
| Contatori di prestazioni | 3 mesi | 15 minuti |
| Eventi di sistema | 3 mesi | 15 minuti |
| Serie Well-Known | 1 anno | 1 ora |


### <a name="aggregation-types"></a>Tipi di aggregazione
Poiché ogni serie di registrare un solo punto dati per ogni intervallo di raccolta, ciascuna serie dispone di un associati al tipo di aggregazione è. La tabella seguente descrive l'origine dati e il corrispondente tipo di aggregazione:

>[!NOTE]
>Per i contatori delle prestazioni, è possibile scegliere da alcuni tipi diversi di aggregazione.

| Origine dati | Tipi di aggregazione |
| --------------- | --------------- |
| Contatori di prestazioni | SUM, average, max, min |
| Eventi di sistema | Conteggio |
| Serie noto del disco | Last (ultimo valore nell'intervallo di raccolta) |
| Serie noto volume | Last (ultimo valore nell'intervallo di raccolta) |
| Serie noto della CPU | Media |
| Serie noto di rete | Media |

## <a name="data-footprint"></a>Impatto dei dati

Sistema Insights raccoglie tutti i dati in locale nell'unità C (c). In generale, l'impatto dei dati di sistema Insights è limitata. Dipende direttamente il tipo e il numero di origini dati che ogni funzionalità specifica e la tabella seguente illustra in dettaglio l'utilizzo dell'archiviazione per ogni tipo di dati:

| Origine dati | Impatto massimo |
| --------------- | --------------- |
| Contatori di prestazioni | 240 KB |
| Eventi di sistema | 200 KB |
| Serie noto del disco | 200 KB per disco |
| Serie noto volume | 300 KB per ogni volume |
| Serie noto della CPU | 100 KB |
| Serie noto di rete | 300 KB per ogni scheda di rete |

>[!NOTE]
>**Per impostazione predefinita le funzionalità di previsione, il footprint di massimo deve essere minore di 10 MB per la maggior parte delle macchine virtuali autonome.** 

## <a name="see-also"></a>Vedere anche
Per altre informazioni su System Insights, usare le risorse seguenti:

- [Panoramica sistema Insights](overview.md)
- [Funzionalità di comprensione](understanding-capabilities.md)
- [La funzionalità di gestione](managing-capabilities.md)
- [Aggiunta e lo sviluppo di funzionalità](adding-and-developing-capabilities.md)
- [Sistema Insights domande frequenti](faq.md)
