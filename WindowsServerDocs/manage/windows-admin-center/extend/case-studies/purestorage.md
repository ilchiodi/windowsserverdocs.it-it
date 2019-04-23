---
title: Case Study di Windows Admin Center SDK - archiviazione Pure
description: Case Study di Windows Admin Center SDK - archiviazione Pure
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849922"
---
# <a name="pure-storage-extension"></a>Estensione di archiviazione pure

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>Offre la gestione End-to-End matrice per Windows Admin Center 

[Archiviazione pure](https://www.purestorage.com/) offre enterprise, soluzioni di archiviazione dati all-flash che recapitano architettura incentrate sui dati per accelerare il tuo business per un vantaggio competitivo.  Pure è un Microsoft Gold Partner, certificati per Microsoft Windows Server e sviluppa le integrazioni tecniche per soluzioni Microsoft, ad esempio Azure, Hyper-V, SQL Server, System Center, Windows PowerShell e Windows SMB. Pure annunciato di recente una Technical preview di un'estensione che supportano la versione più recente di Windows Admin Center che fornisce una visualizzazione a riquadro singolo nei prodotti FlashArray Pure.  Da questa estensione, gli utenti hanno da uno strumento per eseguire attività di monitoraggio, visualizzare le metriche delle prestazioni in tempo reale e gestire gli iniziatori e volumi di archiviazione.

Quando Windows Admin Center era noto come "Progetto Honolulu", Pure in anticipo, visto il valore di essere in grado di fornire ai clienti e partner la possibilità di gestire più FlashArrays archiviazione Pure dall'unico riquadro che include Windows Admin Center.

Quando Pure cominciarono il caso d'uso con "Progetto Honolulu" giunse immediatamente il rischio di fornire un'esperienza di gestione unificata tra Windows Admin Center e FlashArray. Pure collaborato con il team di progettazione Windows Admin Center, che ha consentito di definire i dettagli di implementazione per le funzionalità. Pure era anche in grado di fornire commenti e suggerimenti nelle fasi iniziali di Windows Admin Center e contributi al team di Microsoft. 

![Estensione di archiviazione pure](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>"Sono stati integrati in un set di funzionalità che simula l'interfaccia web FlashArray per abilitare la gestione diretta all'interno di Windows Admin Center. Clienti e partner trarranno vantaggio da un'unica console e che sia necessario utilizzare due strumenti di gestione differente. Oltre al singolo punto di gestione vantaggi i clienti saranno in grado di gestire in base al contesto di Windows Server a cui sono connesse le FlashArray."</cite>
>
> -Integrazione, archiviazione Pure, direttore tecnico soluzioni di Microsoft e Barkz

Le funzionalità incluse nell'estensione di soluzioni di archiviazione Pure includono:
- La connessione a più FlashArrays.
- Visualizzazione dettagli FlashArray, tra cui IOPs, larghezza di banda, latenza, riduzione dei dati e la gestione dello spazio. Questi sono tutti gli stessi dettagli ottenuto la GUI di gestione FlashArray.
- Visualizzare i gruppi di host configurati vengono usati per consentire l'accesso ai volumi condivisi per gli host Windows Server e volumi condivisi cluster (CSV).
- Visualizzazione host, ovvero Tutte le informazioni di connettività è disponibile tra i nomi Host, iSCSI Qualified Name (iqn) e nomi universali (WWN).
- Gestire i volumi, Ciò include la possibilità di creare ed eliminare i volumi. Una volta che viene eliminato un volume verrà inserito nel bucket eliminare definitivamente gli elementi e dovrai Eradicate dalla GUI di gestione FlashArray principale.
- Gestire gli iniziatori, Tratta una delle funzionalità più interessanti che fornisce contesto per i singoli server viene gestito mediante la distribuzione di Windows Admin Center. È possibile visualizzare i dischi (volumi) connessi ai singoli server di Windows, verificare se MultiPath i/o (MPIO) è installato o configurato e la creazione e montaggio nuovi volumi.

Oggetto [video dimostrativo](https://youtu.be/IFAeCAd6V2g) creato che mostra tutte le funzionalità che fornisce l'estensione di soluzioni di archiviazione Pure. 

La seguente schermata illustra la visualizzazione quali dischi (volumi) sono connessi a un host Windows Server specifico. Oltre a visualizzare i dettagli di connettività, controlliamo se Multipath i/o sono configurato.

![Estensione di archiviazione pure](../../media/extend-case-study-purestorage/purestorage-2.png)

Oltre a visualizzare i dischi, i nuovi volumi possono essere creati e immediatamente montati nell'host senza dover usare lo strumento Gestione disco di Windows.

![Estensione di archiviazione pure](../../media/extend-case-study-purestorage/purestorage-3.png)

Dopo il rilascio di anteprima tecnica, ai suggerimenti dei clienti raccolti finora sono stato molto positivo e anche ci ha fornito informazioni dettagliate diverse funzionalità da aggiungere nelle future versioni. 

Risorse aggiuntive:
- [Post di blog annuncio archiviazione estensione pure](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2) podcast
