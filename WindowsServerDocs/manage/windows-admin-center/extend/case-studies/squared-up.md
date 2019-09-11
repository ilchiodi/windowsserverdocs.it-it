---
title: Case Study di Windows Admin Center SDK-quadrato
description: Case Study di Windows Admin Center SDK-quadrato
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 0d4469684ad9cbdadec5c40cb3b5178345b64a6d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865342"
---
# <a name="squared-up-extension"></a>Estensione quadrata

## <a name="bringing-scom-based-monitoring-server-dependency-visibility-and-external-data-insights-into-windows-admin-center"></a>Introduzione del monitoraggio basato su SCOM, visibilità delle dipendenze del server e informazioni dettagliate sui dati esterni nell'interfaccia di amministrazione di Windows

Squared Up è stato creato con la visione dell'uso della visualizzazione dei dati per contribuire alla risoluzione delle difficoltà legate alla complessità IT aziendale. Il software univoco, leggero e solo dell'interfaccia utente si basa su una piattaforma di System Center Operations Manager avanzata di Microsoft, oltre che sull'integrazione con origini dati aggiuntive, dal Log Analytics di Azure, Application Insights e sistema Microsoft. Al centro Service Manager a prodotti di terze parti come ServiceNow, Splunk e molti altri, per offrire visibilità in infrastrutture aziendali su larga scala e di applicazioni, sia locali che tra ambienti cloud ibridi.

> <cite>"Abbiamo utilizzato in modo intensivo l'interfaccia di amministrazione di Windows per tutta la sua versione Technical Preview ed è stata già una grande hit, contribuendo a risolvere le difficoltà, ad esempio i nostri tecnici, che hanno semplificato l'accesso ai nostri laboratori di configurazione e abbiamo intenzione di renderla la nostra gestione principale una volta rilevata la versione completa. Ci piace il potenziale dell'integrazione con il quadrato e la possibilità di esporre tutti i dati in un'unica posizione. "</cite>
>
> --David Acevedo, specialista I/S presso NuStar Energy L.P.

I client quadrati sono in grado di gestire centinaia, spesso migliaia, di server Windows e dei diversi portfolio di applicazioni offerti da tali server, oltre a essere affiancati e Microsoft in una missione per offrire ai team IT il meglio possibile nell'interfaccia utente Web rapida e moderna per fornire le informazioni necessarie. Di conseguenza, il team al quadrato ha subito un interessante allineamento con l'interfaccia di amministrazione di Windows che riporta gli stessi valori e le stesse entità alla prossima generazione di amministrazione di Windows Server. In particolare, il team crede che i dati sulle prestazioni a lungo termine, le informazioni dettagliate sulle dipendenze del server in tempo reale e il contesto dell'applicazione esposti da Squared siano perfettamente complementari alle funzionalità di gestione dei dati e dei server in tempo reale fornite da Interfaccia di amministrazione di Windows.

![Estensione quadrata](../../media/extend-case-study-squared-up/squared-up-1.png)

> <cite>"In qualità di organizzazione che gestisce un server su larga scala, l'integrazione quadrata/interfaccia di amministrazione di Windows rappresenta il matrimonio perfetto tra gli strumenti localizzati e centralizzati e come la possibilità di generare un server direttamente in modalità di manutenzione dall'interno di L'interfaccia di amministrazione di Windows è un'ottima piccola conquista per noi</cite>
>
> -– Kip Granson, amministratore dei sistemi di virtualizzazione presso Purdue University

Con una visione chiara di voler presentare facilmente i dati all'interno dell'interfaccia di amministrazione di Windows, è stata affiancata con la prima versione di anteprima privata di Windows Admin Center SDK ed è stata individuata in modo semplice, ben documentato e flessibile.

L'uso di Windows Admin Center SDK, Squared Up è stato in grado di creare un'estensione che incorpora in modo dinamico le visualizzazioni quadrate pertinenti nell'esperienza dell'interfaccia di amministrazione di Windows. Ad esempio, all'interno del contesto di un server o di un cluster specifico, le visualizzazioni quadrate vengono automaticamente incorporate per fornire visibilità estesa. Le visualizzazioni includono le tendenze cronologiche delle metriche delle prestazioni e della capacità principali (ad esempio CPU, memoria e disco), lo stack di hosting (piattaforma cloud o virtualizzazione del Data Center), i componenti dell'applicazione come i database e i servizi SQL e anche log Analytics basato sul cloud e ITSM i dati.

![Estensione quadrata](../../media/extend-case-study-squared-up/squared-up-2.png)

Squared Up e l'interfaccia di amministrazione di Windows condividono un'architettura web moderna e un ethos di progettazione, che ha consentito una semplice integrazione tecnica e un'esperienza utente uniforme. Grazie all'amministrazione basata sul Web che diventa sempre più la norma, crediamo che questo metodo di integrazione tra sistemi diversi sia la chiave per sbloccare un'esperienza di amministrazione moderna e unificata.

> <cite>"Vediamo l'interfaccia di amministrazione di Windows come l'avanguardia dell'amministrazione moderna di Windows Server, quindi è stata un'ottima esperienza per lavorare a stretto contatto con il team e il fatto che lavorano con tale velocità, entusiasmo, flessibilità e all'interno di questi concetti i paradigmi di sviluppo moderni li hanno reso molto adatti con il modo in cui noi, una società di sviluppo software snella, agile e veloce, lavorarci ".</cite>
>
> --Richard Benwell, progettista del prodotto al quadrato

Da questo allineamento naturale, il team di sviluppo al quadrato è stato in grado di raggiungere rapidamente l'integrazione di un prototipo, visualizzando il quadrato in modo nativo all'interno dell'esperienza dell'interfaccia di amministrazione di Windows e di inserirlo in mano ai propri esordi, tecnici client di anteprima. Dalle reazioni dei clienti, è stato immediatamente chiaro che la storia era vincente.

> <cite>"Uno dei principali problemi di gestione del servizio in attesa nel nostro ambiente di oltre 3.500 server è l'unificazione dei diversi scenari di gestione e monitoraggio degli strumenti e l'integrazione tra il quadrato e l'interfaccia di amministrazione di Windows, che offre insieme a un numero così elevato di dati, da un numero così elevato di origini diverse, in un'unica console, è molto grande per noi. "</cite>
>
> --Martin Ehrnst, responsabile tecnico per Azure alle Intility a/S

Con questo tipo di entusiasmo da un quadrato di client già e con moltissime nuove funzionalità ancora disponibili nell'interfaccia di amministrazione di Windows, il quadrato è molto entusiasta del futuro di questa integrazione e delle straordinarie possibilità che si apre per i loro clienti e si tratta di un vero e proprio riquadro per la gestione delle operazioni IT.

L'integrazione con il centro di amministrazione di Windows e il quadrato è attualmente in versione beta. per informazioni dettagliate, vedere la [pagina dedicata](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-wac&utm_medium=public-relations&utm_campaign=honolulu) per l'accesso. Se l'organizzazione usa Microsoft System Center Operations Manager e non è ancora stata eseguita la quadratura, che è essenziale per il funzionamento dell'estensione, è anche possibile ottenere una versione di valutazione gratuita di 30 giorni con funzionalità complete dalla stessa posizione. 
