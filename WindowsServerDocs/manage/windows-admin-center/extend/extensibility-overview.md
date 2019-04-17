---
title: Estensioni per Windows Admin Center
description: Estensioni per Windows Admin Center SDK (Project Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fa3d7e75b32f0195346e58db54b7932c8d2fd3b9
ms.sourcegitcommit: 659544db1e19d6eecc52c7de07116ae735280544
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2019
ms.locfileid: "9001843"
---
# Estensioni per Windows Admin Center

>Si applica a: Windows Admin Center, Anteprima di Windows Admin Center

Windows Admin Center è creato come una piattaforma estendibile che consente a partner e sviluppatori di sfruttare le funzionalità esistenti in Windows Admin Center, integrare facilmente altri prodotti e soluzioni per l'amministrazione IT e offrire un valore aggiunto ai clienti. Tutte le soluzioni e gli strumenti in Windows Admin Center sono creati come estensione e usano le stesse funzionalità di estendibilità disponibili per i partner e gli sviluppatori, così puoi creare strumenti potenti quanto quelli attualmente disponibili in Windows Admin Center.

Le estensioni di Windows Admin Center sono state create con tecnologie Web moderne, tra cui HTML5, CSS, Angular, TypeScript e jQuery e possono gestire i server di destinazione tramite PowerShell o WMI. Puoi anche gestire server di destinazione, servizi o dispositivi con protocolli diversi, ad esempio REST, creando un plug-in del gateway di Windows Admin Center.

## Perché dovresti sviluppare un'estensione di Windows Admin Center?

Sviluppando le estensioni per Windows Admin Center, puoi migliorare il prodotto e offrire più valore ai clienti, ad esempio grazie ai vantaggi seguenti:

- **Integrazione con gli strumenti Windows Admin Center:** integra i tuoi prodotti e servizi con gli strumenti di gestione di server e cluster in Windows Admin Center e offri ai clienti funzionalità unificate e ottimizzate di monitoraggio end-to-end, gestione e risoluzione dei problemi.
- **Uso delle funzionalità di sicurezza, identità e gestione della piattaforma:** abilita il supporto di Azure Active Directory (AAD), Multi-Factor Authentication, il controllo degli accessi in base al ruolo, la registrazione e il controllo per i prodotti e i servizi usando le funzionalità della piattaforma Windows Admin Center per soddisfare i complessi requisiti delle organizzazioni IT moderne.
- **Sviluppo con le tecnologie Web più recenti:** crea rapidamente straordinarie esperienze utente con le tecnologie Web moderne, tra cui HTML5, CSS, Angular, TypeScript e jQuery, e l'ampia gamma di potenti controlli dell'interfaccia utente inclusi in Windows Admin Center SDK.
- **Estensione del raggio del prodotto:** diventa parte integrante del nuovo ecosistema di Windows Admin Center raggiungendo la nostra base di clienti in rapida crescita e sfruttando l'evento del lancio di Windows Server 2019 che avverrà nei prossimi mesi.

## Iniziare a sviluppare con Windows Admin Center SDK

Introduzione allo sviluppo di Windows Admin Center è facile.  Codice di esempio può essere disponibile per i tipi di estensione [dello strumento](develop-tool.md), [soluzioni](develop-solution.md)e [plug-in gateway](develop-gateway-plugin.md) nella nostra documentazione SDK. Ci si useranno CLI Windows Admin Center per creare un nuovo progetto di estensione e quindi seguire le indicazioni per personalizzare il progetto per soddisfare le esigenze individuali.

Abbiamo impostato un Windows Admin Center [SDK toolkit di progettazione](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) disponibili per aiutarti a rapidamente simulare le estensioni in PowerPoint usando stili Windows Admin Center, controlli e modelli di pagina. Vedi l'estensione l'aspetto in Windows Admin Center prima di iniziare a scrivere codice!

Abbiamo anche il codice di esempio ospitato su GitHub: [Strumenti di sviluppo](https://aka.ms/wacsdk) è un'estensione della soluzione esempio che contiene un insieme completo di controlli che puoi cercare e utilizzare in un'estensione personalizzata. Strumenti di sviluppo è un'estensione completamente funzionante che può essere trasferita localmente in Windows Admin Center in modalità di sviluppo.

Vedi gli argomenti riportati di seguito per altre informazioni su SDK e su come iniziare:

- [Informazioni sul funzionamento delle estensioni](understand-extensions.md)
- [Sviluppare un'estensione](developing-extensions.md)
- [Guide](guides.md)
- [Pubblicare l'estensione](publish-extensions.md)

## Partner in evidenza

Scopri l'enorme valore che i nostri partner hanno iniziato ad apportare all'ecosistema di Windows Admin Center e prova subito le loro estensioni. Altre informazioni su [come installare le estensioni](../configure/using-extensions.md) da Windows Admin Center.

### DataON

Estensione DataON introduce il monitoraggio, gestione ed end-to-end approfondimenti iperconvergente dell'infrastruttura e archiviazione sistemi DataON basati su Windows Server. L'estensione aggiunge il valore univoco, ad esempio report dei dati cronologici, mapping del disco, avvisi di sistema e servizio di casa chiamata SAN simili, integrano il server Windows Admin Center e le funzionalità di gestione dell'infrastruttura iperconvergente con un' esperienza unificata. [Altre informazioni sull'estensione DataON MUST e sull'esperienza di sviluppo](case-studies/dataon.md).

![Estensione DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### Fujitsu

Le estensioni Fujitsu ServerView Health e RAID Health per Windows Admin Center forniscono funzionalità di gestione e monitoraggio approfonditi dei componenti hardware critici per i server Fujitsu PRIMERGY, ad esempio processori, memoria, sottosistemi di alimentazione e archiviazione. Usando i controlli dell'interfaccia utente e i modelli di progettazione UX di Windows Admin Center, Fujitsu ha consentito un enorme avanzamento verso la nostra visione di una comprensione end-to-end di servizi e ruoli server, del sistema operativo e della gestione dell'hardware grazie alla piattaforma Windows Admin Center. [Altre informazioni sulle estensioni Fujitsu e sull'esperienza di sviluppo](case-studies/fujitsu.md).

![Estensione Fujitsu ServerView](../media/extensibility-overview/fujitsu-serverview-extension.png)

### Lenovo

Estensione XClarity Integrator del Lenovo accetta la gestione dell'hardware al livello successivo integrando in varie esperienze in Windows Admin Center. La soluzione XClarity Integrator fornisce una panoramica generale di tutti i server di Lenovo ed estensioni dello strumento diversi forniscono informazioni dettagliate di hardware, se si è connessi a un unico server, cluster di failover o un cluster iper-convergenti. [Ulteriori informazioni sull'estensione Lenovo XClarity Integrator](case-studies/lenovo.md).

![Estensione di Lenovo](../media/extensibility-overview/lenovo-extension.png)

### Archiviazione pura

Archiviazione pura offre enterprise, soluzioni di archiviazione di dati all-flash che offrono architettura incentrato sui dati per accelera il business per un vantaggio competitivo. L'estensione di archiviazione pura per Windows Admin Center offre un unico riquadro nei prodotti FlashArray pura e consente agli utenti di eseguire attività di monitoraggio, visualizzare le metriche delle prestazioni in tempo reale e gestire volumi di archiviazione e iniziatori tramite una singola interfaccia utente esperienza. [Ulteriori informazioni sulle estensioni del Pure e l'esperienza di sviluppo](case-studies/purestorage.md).

![Estensione di archiviazione pura](../media/extensibility-overview/purestorage-extension.png)

### Squared Up

Squared Up offre la migliore esperienza di monitoraggio del settore, basata su System Center Operations Manager e integrata con Azure Log Analytics, Application Insights e altre soluzioni di monitoraggio. L'[estensione Squared Up](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu) porta i dati cronologici sulle prestazioni e le topologie e le dipendenze delle applicazioni live nel contesto della gestione di server e cluster offerta da Windows Admin Center e i primi clienti hanno confermato l'enorme vantaggio di poter disporre di un'immensa quantità di dati provenienti da fonti diverse in un'unica posizione. [Altre informazioni sull'estensione Squared Up e sull'esperienza di sviluppo](case-studies/squared-up.md).

![Estensione Squared Up](../media/extensibility-overview/squaredup-extension.png)