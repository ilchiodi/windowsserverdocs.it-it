---
title: Case Study di Windows Admin Center SDK-archiviazione pure
description: Case Study di Windows Admin Center SDK-archiviazione pure
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: cfd9d31d388b9acb1a4a4fa40b3975b235a8634b
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546615"
---
# <a name="pure-storage-extension"></a>Estensione di archiviazione pure

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>Fornire la gestione di matrici end-to-end per l'interfaccia di amministrazione di Windows 

L' [archiviazione pure](https://www.purestorage.com/) offre soluzioni di archiviazione dei dati aziendali e di tutti i flash che offrono un'architettura incentrata sui dati per accelerare l'attività aziendale per un vantaggio competitivo.  Pure è un partner Microsoft Gold, Certified per Microsoft Windows Server, e sviluppa integrazioni tecniche per soluzioni Microsoft principali, ad esempio Azure, Hyper-V, SQL Server, System Center, Windows PowerShell e Windows SMB. Pure ha recentemente annunciato un'anteprima tecnica di un'estensione che supporta la versione più recente dell'interfaccia di amministrazione di Windows che offre una visualizzazione a un singolo riquadro dei prodotti FlashArray puri.  Da questa estensione, gli utenti sono abilitati da uno strumento per eseguire attività di monitoraggio, visualizzare le metriche delle prestazioni in tempo reale e gestire i volumi e gli iniziatori di archiviazione.

All'inizio, quando l'interfaccia di amministrazione di Windows era nota come "progetto Honolulu", ha visto il valore di poter fornire ai clienti e ai partner la possibilità di gestire più FlashArrays di archiviazione pure da un unico riquadro di vetro fornito dal centro di amministrazione di Windows.

Quando pure ha iniziato a cercare il caso d'uso con "Project Honolulu", ha subito la possibilità di fornire un'esperienza di gestione unificata tra l'interfaccia di amministrazione di Windows e FlashArray. Pure strettamente collaborato con il team di progettazione di Windows Admin Center, che ha consentito di definire i dettagli di implementazione per le funzionalità. Pure è stato inoltre in grado di fornire commenti e suggerimenti nelle prime fasi dell'interfaccia di amministrazione di Windows e contribuire al team Microsoft. 

![Estensione di archiviazione pure](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>"Abbiamo integrato un set di funzionalità che simula l'interfaccia Web di FlashArray per abilitare la gestione diretta dal centro di amministrazione di Windows. I nostri clienti e i nostri partner trarranno vantaggio da un unico riquadro di vetro rispetto alla necessità di lavorare con due diversi strumenti di gestione. Oltre ai vantaggi del singolo punto di gestione, i clienti saranno in grado di gestire in modo contestuale i server Windows connessi alla FlashArray ".</cite>
>
> --Barkz, Technical Director Microsoft Solutions & Integration, pure storage

Le funzionalità incluse nell'estensione della soluzione di archiviazione pura includono:
- Connessione a più FlashArrays.
- Visualizzazione dei dettagli di FlashArray, tra cui IOPs, larghezza di banda, latenza, riduzione dei dati e gestione dello spazio. Questi sono tutti gli stessi dettagli che si ottengono dalla GUI di gestione di FlashArray.
- Consente di visualizzare i gruppi host configurati utilizzati per abilitare l'accesso ai volumi condivisi per gli host Windows Server e i volumi condivisi cluster (CSVs).
- Visualizza host: tutte le informazioni di connettività sono disponibili, inclusi i nomi host, il nome qualificato iSCSI (IQN) e i nomi universali (nomi WWN).
- Gestire i volumi, inclusa la possibilità di creare ed eliminare i volumi. Una volta eliminato, il volume verrà inserito nel bucket degli elementi eliminati e sarà necessario sradicarlo dalla GUI principale di gestione di FlashArray.
- Gestisci iniziatori: questa è una delle funzionalità più interessanti che forniscono il contesto ai singoli server gestiti dalla distribuzione del centro di amministrazione di Windows. È possibile visualizzare i dischi connessi (volumi) per i singoli server Windows, verificare se la funzionalità multipath-IO (MPIO) è installata/configurata e creare o montare nuovi volumi.

È stato creato un [video](https://youtu.be/IFAeCAd6V2g) dimostrativo che Mostra tutte le funzionalità fornite dall'estensione della soluzione di archiviazione pura. 

Lo screenshot seguente illustra la visualizzazione dei dischi (volumi) connessi a un host Windows Server specifico. Oltre a visualizzare i dettagli relativi alla connettività, viene verificato se la configurazione multipath-IO è configurata.

![Estensione di archiviazione pure](../../media/extend-case-study-purestorage/purestorage-2.png)

Oltre a visualizzare i dischi, è possibile creare nuovi volumi e montarli immediatamente nell'host senza dover utilizzare lo strumento Gestione disco di Windows.

![Estensione di archiviazione pure](../../media/extend-case-study-purestorage/purestorage-3.png)

Dal rilascio della versione Technical Preview, i commenti e suggerimenti dei clienti finora sono stati molto positivi e hanno fornito informazioni approfondite sulle diverse funzionalità da aggiungere nelle versioni future. 

Risorse aggiuntive:
- [Post di Blog sull'annuncio dell'estensione di archiviazione pure](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- Podcast [PureReport](https://itunes.apple.com/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)
