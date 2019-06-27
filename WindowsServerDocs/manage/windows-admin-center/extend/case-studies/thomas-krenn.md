---
title: Case Study di Windows Admin Center SDK - Thomas Krenn
description: Case Study di Windows Admin Center SDK - Thomas Krenn
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/24/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93b8a450aa86a454ec6febd349fcaa35df590266
ms.sourcegitcommit: 3be280c8638214857dc355b201eb56a04499a5e5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2019
ms.locfileid: "67396761"
---
# <a name="thomas-krennag-extension"></a>Estensione di Thomas Krenn.AG

## <a name="intuitive-server-and-storage-health-management"></a>Gestione di integrità di server e archiviazione intuitiva

L'estensione di Thomas Krenn.AG Windows Admin Center è progettato specificamente per i 2 nodi a disponibilità elevata [Micro-Cluster di S2D](https://www.thomas-krenn.com/en/products/application/software-defined-storage/s2d-micro-cluster.html) appliance. L'interfaccia web con interfaccia grafica intuitiva Visualizza lo stato di integrità Micro-di un Cluster tramite un dashboard semplice e consente di eseguire il drill-su dispositivi di archiviazione, interfacce di rete o l'intero cluster per visualizzare altri dettagli.

L'estensione fornisce un accesso intuitivo alle informazioni in genere necessari per primo livello del servizio e il supporto chiamate, ad esempio i numeri di serie, le versioni del software, utilizzo spazio di archiviazione e altro ancora. È progettato per essere utile agli amministratori che non dispongono di alcuna esperienza precedente con l'infrastruttura iperconvergente di Windows Server.

Alcune delle informazioni disponibili sono:
- Informazioni generali su Micro-nodi e i Micro-Cluster
- Sistema operativo / stato del dispositivo di avvio
- Capacità di unità disco rigido e la memorizzazione nella cache dello stato unità SSD
- Eventi cluster
- Informazioni e stato della rete

Usare il dashboard per determinare lo stato di integrità del cluster e informazioni di sistema importanti, ad esempio i numeri di serie, modello, versione del sistema operativo e utilizzo. Inoltre, fan, interfaccia di rete e integrità hardware nodo Generale vengono visualizzati nel dashboard anche.

![Estensione di Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-1.png)

È possibile risalire alle periferiche di archiviazione per visualizzare i numeri di serie SMART-status e utilizzo della capacità. Periferiche di avvio mostrano anche wear out indicatori, riallocata i settori e risparmio energia sul tempo, quali sono i migliori indicatori di integrità unità SSD.

![Estensione di Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-2.png)

L'icona dello stato del cluster viene espansa per visualizzare un riepilogo dei dettagli operativi del cluster.

![Estensione di Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-3.png)

Dopo del cloud di Azure di controllo del Cluster-Micro è non disponibile per un'intera notte, colpo è sufficiente per identificare il problema. Facendo clic su "Notifiche" immediatamente Elenca gli eventi rilevanti per la correzione rapida. Eventi cluster sono localizzati e determinati dalla lingua del sistema operativo base. L'estensione stessa supporta l'inglese e tedesco.

![Estensione di Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-4.png)

Informazioni di rete sono disponibili anche.

![Estensione di Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-5.png)

In base ai suggerimenti dei clienti, abbiamo implementato anche "Modalità scura" disponibile in Windows Admin Center v1904. Si tratta di soothing nei data center scuro e in archivi illuminati in modo inadeguato e più vicina. Rende anche Windows Admin Center più accessibile, riducendo il riflesso per gli amministratori con determinati problemi visivi.

![Estensione di Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-6.png)

Thomas Krenn resi conto immediatamente che l'usabilità e accessibilità per gli amministratori non sottoposto a training sarebbe chiave per un'esperienza ottimale ai clienti per infrastruttura iperconvergente sul mercato di aziende di piccole e medie dimensioni. Estensione di Micro-Cluster del Thomas-Krenn integra perfettamente funzionalità di gestione di Windows Admin Center native uomo includendo informazioni hardware proprietario del dashboard e raggruppamento nuovamente le informazioni di integrità importanti del cluster in una nuova, Converto l'interfaccia.

Durante il processo di sviluppo è stato deciso di distribuire Windows Admin Center 1904 in una configurazione a disponibilità elevata nel cluster stesso, assicurandosi di gestibilità anche dopo gli errori dei nodi. L'estensione preinstallata, proprio come l'intero sistema operativo.

L'estensione è stato compilato in parallelo con Windows Admin Center 1904 sviluppate presso Microsoft. Stretta collaborazione e problemi di feedback continuo esposte su entrambi i lati che sono stati risolti congiuntamente prima il prodotto è stato avviato nel mese di aprile 2019. Thomas Krenn è estremamente fiero di essere uno dei primi a completamente supportano e implementano nuove funzionalità di Windows Admin Center 1904.
