---
title: Case Study di Windows Admin Center SDK - archiviazione pura
description: Case Study di Windows Admin Center SDK - archiviazione pura
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: ebeec824f802f020d0ece17524ba43b1baeba893
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/08/2019
ms.locfileid: "8995368"
---
# Estensione di archiviazione pura

## Forniscono la gestione di matrice End-to-End per Windows Admin Center 

[Archiviazione pura](https://www.purestorage.com/) offre enterprise, soluzioni di archiviazione di dati all-flash che offrono architettura incentrato sui dati per accelera il business per un vantaggio competitivo.  Pure è Microsoft Gold Partner, ottenere la certificazione per Microsoft Windows Server e sviluppa integrazione tecniche per le principali soluzioni di Microsoft, ad esempio Azure, Hyper-V, SQL Server, System Center, Windows PowerShell e SMB di Windows. Pure annunciato di recente un'anteprima tecnica di un'estensione che supporta la versione più recente di Windows Admin Center che fornisce una visualizzazione a singolo riquadro nei prodotti FlashArray pura.  Da questa estensione, gli utenti avranno da uno strumento per eseguire attività di monitoraggio, visualizzare le metriche delle prestazioni in tempo reale e gestire iniziatori e i volumi di archiviazione.

Inizialmente, quando Windows Admin Center era noto come "Project Honolulu", Pure visto il valore di essere in grado di fornire ai clienti e partner la possibilità di gestire più FlashArrays archiviazione pura nel riquadro di vetro che Windows Admin Center fornisce singolo.

Quando inizia a Pure cercando il caso di utilizzo con "Project Honolulu" sono realizzati immediatamente il rischio di fornire un'esperienza di gestione unificata tra Windows Admin Center e FlashArray. Pure collaborato con il team di progettazione di Windows Admin Center, che ha consentito di definire i dettagli di implementazione per le funzionalità. Pure anche è stato in grado di fornire feedback nelle prime fasi di Windows Admin Center e contributi al team di Microsoft. 

![Estensione di archiviazione pura](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>"Abbiamo integrato un set di funzionalità che simula l'interfaccia web FlashArray per abilitare la gestione diretta all'interno di Windows Admin Center. I clienti e i partner trarranno vantaggio da un unico riquadro di vetro rispetto alla necessità di lavorare con due strumenti di gestione diversi. Oltre al singolo punto di gestione dei vantaggi clienti potranno contestualmente gestire Windows Server connessi al FlashArray."</cite>
>
> -Barkz, le soluzioni Microsoft Technical Director e integrazione di archiviazione pura

Le funzionalità incluse nell'estensione della soluzione di archiviazione pura includono:
- Connessione a più FlashArrays.
- I dettagli FlashArray, tra cui IOPs, larghezza di banda, latenza, la riduzione dei dati e alla gestione dello spazio di visualizzazione. Questi sono tutti i dettagli stesso, che ottenere dall'interfaccia utente di gestione della FlashArray.
- Visualizza configurato gruppi di host che vengono utilizzati per abilitare l'accesso di volume condiviso per gli host di Windows Server e i volumi condivisi cluster (volumi condivisi cluster).
- Gli host di visualizzazione, ovvero Tutte le informazioni di connettività è disponibile, inclusi i nomi Host, iSCSI nome qualificato (nomi) e nomi universali (WWN).
- Gestire volumi, Inclusa la possibilità di creazione e l'eliminazione di volumi. Una volta che viene eliminato un volume verrà posizionato nell'intervallo di elementi distruggere e dovrai Eradicate da GUI di gestione FlashArray principale.
- Gestire iniziatori, Questo è una delle caratteristiche più interessanti fornire il contesto dei singoli server gestiti dalla distribuzione di Windows Admin Center. È possibile visualizzare i dischi collegati (volumi) ai singoli Windows Server, controlla se MultiPath-operazioni i/o (MPIO) è installato o configurato e creazione/montaggio nuovi volumi.

È stata creata una [dimostrazione video](https://youtu.be/IFAeCAd6V2g) che mostra tutte le funzionalità che fornisce l'estensione della soluzione di archiviazione pura. 

Il seguente screenshot illustra la visualizzazione quali dischi (volumi) sono connessi a un host specifico di Windows Server. Oltre a visualizzare i dettagli di connettività, controlliamo se Multipath-IO è configurato.

![Estensione di archiviazione pura](../../media/extend-case-study-purestorage/purestorage-2.png)

Oltre a visualizzare i dischi, i nuovi volumi possono essere creati e montati immediatamente all'host senza usare lo strumento di Gestione disco di Windows.

![Estensione di archiviazione pura](../../media/extend-case-study-purestorage/purestorage-3.png)

Poiché il rilascio nostro Technical Preview, il feedback dei clienti raccolti finora è stata molto positivo e anche ci ha fornito approfondimenti diverse funzionalità per aggiungere nelle future versioni. 

Risorse aggiuntive:
- [Post di blog annuncio archiviazione estensione puro](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- Podcast [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)
