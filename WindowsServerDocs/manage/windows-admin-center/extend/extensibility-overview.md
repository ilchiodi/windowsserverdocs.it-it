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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885002"
---
# <a name="extensions-for-windows-admin-center"></a>Estensioni per Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Windows Admin Center è creato come una piattaforma estendibile che consente a partner e sviluppatori di sfruttare le funzionalità esistenti in Windows Admin Center, integrare facilmente altri prodotti e soluzioni per l'amministrazione IT e offrire un valore aggiunto ai clienti. Tutte le soluzioni e gli strumenti in Windows Admin Center sono creati come estensione e usano le stesse funzionalità di estendibilità disponibili per i partner e gli sviluppatori, così puoi creare strumenti potenti quanto quelli attualmente disponibili in Windows Admin Center.

Le estensioni di Windows Admin Center sono state create con tecnologie Web moderne, tra cui HTML5, CSS, Angular, TypeScript e jQuery e possono gestire i server di destinazione tramite PowerShell o WMI. Puoi anche gestire server di destinazione, servizi o dispositivi con protocolli diversi, ad esempio REST, creando un plug-in del gateway di Windows Admin Center.

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>Perché dovresti sviluppare un'estensione di Windows Admin Center?

Sviluppando le estensioni per Windows Admin Center, puoi migliorare il prodotto e offrire più valore ai clienti, ad esempio grazie ai vantaggi seguenti:

- **Integrare con strumenti di Windows Admin Center:** Integra i prodotti e servizi con gli strumenti di gestione di server e cluster in Windows Admin Center e potrai distribuire unificata e uniforme, end-to-end di monitoraggio, gestione, risoluzione dei problemi relativi ai tuoi clienti esperienze.
- **È possibile sfruttare le funzionalità di sicurezza, identità e gestione della piattaforma:** Abilitare il supporto di Azure Active Directory (AAD), multi-Factor Authentication, controllo di accesso basato sui ruoli (RBAC), la registrazione, il controllo per i servizi e prodotti sfruttando le funzionalità della piattaforma Windows Admin Center per soddisfare i requisiti complessi degli attuali Organizzazioni IT.
- **Sviluppare usando le tecnologie web più recenti:** Creare rapidamente esperienze utente straordinarie usando tecnologie web moderne incluse HTML5, CSS, Angular, TypeScript e jQuery e i controlli dell'interfaccia utente completo e avanzati inclusi in Windows Admin Center SDK.
- **Estendere outreach prodotto:** Diventano una parte dell'ecosistema Windows Admin Center nuova con outreach ai nostri base clienti in rapida crescita e sfrutta il 2019 Server Windows avviare momentum quest'anno.

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>Inizia a sviluppare con il SDK di Windows Admin Center

Introduzione allo sviluppo di Windows Admin Center è facile!  Codice di esempio è disponibile per [tool](develop-tool.md), [soluzione](develop-solution.md), e [plug-in gateway](develop-gateway-plugin.md) tipi di estensione nella documentazione SDK. Non esiste è possibile usare l'interfaccia CLI di Windows Admin Center per generare un nuovo progetto di estensione e quindi seguire le guide per personalizzare il progetto per soddisfare le esigenze individuali.

Sono state apportate una Windows Admin Center [toolkit progettazione SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) disponibili che consentono di simulare rapidamente le estensioni in PowerPoint con stili di Windows Admin Center, controlli e modelli di pagina. Vedere l'estensione può come appaiono nel Windows Admin Center prima di iniziare a scrivere codice!

Abbiamo anche codice di esempio ospitata in GitHub: [Strumenti di sviluppo](https://aka.ms/wacsdk) è un'estensione di soluzioni di esempio contenente un insieme avanzato di controlli che è possibile esplorare e utilizzare in un'estensione personalizzata. Strumenti di sviluppo è un'estensione completamente funzionante che può essere trasferita localmente in Windows Admin Center in modalità di sviluppo.

Vedi gli argomenti riportati di seguito per altre informazioni su SDK e su come iniziare:

- [Informazioni sul funzionano delle estensioni](understand-extensions.md)
- [Sviluppare un'estensione](developing-extensions.md)
- [Guide](guides.md)
- [Pubblicare l'estensione](publish-extensions.md)

## <a name="partner-spotlight"></a>Partner in evidenza

Scopri l'enorme valore che i nostri partner hanno iniziato ad apportare all'ecosistema di Windows Admin Center e prova subito le loro estensioni. Altre informazioni su [come installare le estensioni](../configure/using-extensions.md) da Windows Admin Center.

### <a name="dataon"></a>DataON

Estensione deve della DataON porta di monitoraggio, gestione ed end-to-end approfondite archiviazione e infrastruttura di sistemi iperconvergenti del DataON basato su Windows Server. L'estensione deve aggiunge valore univoco, ad esempio report sui dati cronologici, mapping del disco, gli avvisi di sistema e servizio home chiamata simile a SAN, arricchire le funzionalità di gestione dell'infrastruttura iperconvergente, tramite un facile, server Windows Admin Center esperienza unificata. [Altre informazioni sull'estensione DataON MUST e sull'esperienza di sviluppo](case-studies/dataon.md).

![Estensione DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

Fujitsu ServerView integrità e le estensioni di integrità di RAID per Windows Admin Center forniscono approfondita di monitoraggio e gestione dei componenti hardware importanti, ad esempio processori, memoria, sottosistemi di archiviazione e potenza per i server di Fujitsu PRIMERGY. Usando i controlli dell'interfaccia utente e i modelli di progettazione UX di Windows Admin Center, Fujitsu ha consentito un enorme avanzamento verso la nostra visione di una comprensione end-to-end di servizi e ruoli server, del sistema operativo e della gestione dell'hardware grazie alla piattaforma Windows Admin Center. [Altre informazioni sulle estensioni Fujitsu e sull'esperienza di sviluppo](case-studies/fujitsu.md).

![Estensione Fujitsu ServerView](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

Estensione XClarity integratore di Lenovo accetta la gestione dell'hardware per il livello successivo grazie alla perfetta integrazione nelle esperienze diverse all'interno di Windows Admin Center. La soluzione Integrator XClarity offre una visualizzazione generale di tutti i server di Lenovo e le estensioni di strumenti diversi forniscono i dettagli sull'hardware se si è connessi a un singolo server, cluster di failover o un cluster iperconvergente. [Altre informazioni sull'estensione Lenovo XClarity Integrator](case-studies/lenovo.md).

![Estensione di Lenovo](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

Archiviazione pure offre enterprise, soluzioni di archiviazione dati all-flash che recapitano architettura incentrate sui dati per accelerare il tuo business per un vantaggio competitivo. L'estensione di archiviazione Pure per Windows Admin Center offre una visualizzazione a riquadro singolo prodotti FlashArray Pure e permette agli utenti di eseguire attività di monitoraggio, visualizzare le metriche delle prestazioni in tempo reale e gestire volumi di archiviazione e gli iniziatori tramite un'unica interfaccia utente esperienza. [Altre informazioni sulle estensioni del Pure e l'esperienza di sviluppo](case-studies/purestorage.md).

![Estensione di archiviazione pure](../media/extensibility-overview/purestorage-extension.png)

### <a name="squared-up"></a>Squared Up

Squared Up offre la migliore esperienza di monitoraggio del settore, basata su System Center Operations Manager e integrata con Azure Log Analytics, Application Insights e altre soluzioni di monitoraggio. L'[estensione Squared Up](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu) porta i dati cronologici sulle prestazioni e le topologie e le dipendenze delle applicazioni live nel contesto della gestione di server e cluster offerta da Windows Admin Center e i primi clienti hanno confermato l'enorme vantaggio di poter disporre di un'immensa quantità di dati provenienti da fonti diverse in un'unica posizione. [Altre informazioni sull'estensione Squared Up e sull'esperienza di sviluppo](case-studies/squared-up.md).

![Estensione Squared Up](../media/extensibility-overview/squaredup-extension.png)