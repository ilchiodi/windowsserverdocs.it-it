---
title: Estensioni per Windows Admin Center
description: Estensioni per Windows Admin Center SDK (Project Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: ee8c0203be25b30f173b1887de506844d5b58738
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406919"
---
# <a name="extensions-for-windows-admin-center"></a>Estensioni per Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center è creato come una piattaforma estendibile che consente a partner e sviluppatori di sfruttare le funzionalità esistenti in Windows Admin Center, integrare facilmente altri prodotti e soluzioni per l'amministrazione IT e offrire un valore aggiunto ai clienti. Tutte le soluzioni e gli strumenti in Windows Admin Center sono creati come estensione e usano le stesse funzionalità di estendibilità disponibili per i partner e gli sviluppatori, così puoi creare strumenti potenti quanto quelli attualmente disponibili in Windows Admin Center.

Le estensioni di Windows Admin Center sono state create con tecnologie Web moderne, tra cui HTML5, CSS, Angular, TypeScript e jQuery e possono gestire i server di destinazione tramite PowerShell o WMI. Puoi anche gestire server di destinazione, servizi o dispositivi con protocolli diversi, ad esempio REST, creando un plug-in del gateway di Windows Admin Center.

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>Perché dovresti sviluppare un'estensione di Windows Admin Center?

Questo è il valore che è possibile portare al prodotto e ai clienti sviluppando estensioni per l'interfaccia di amministrazione di Windows:

- **Eseguire l'integrazione con gli strumenti di amministrazione di Windows:** Integra i tuoi prodotti e servizi con gli strumenti di gestione di server e cluster nell'interfaccia di amministrazione di Windows e Distribuisci le esperienze di monitoraggio, gestione e risoluzione dei problemi unificate e senza problemi ai tuoi clienti.
- **Sfruttare le funzionalità di sicurezza, identità e gestione della piattaforma:** Abilita il supporto di Azure Active Directory (AAD), Multi-Factor Authentication, il controllo degli accessi in base al ruolo (RBAC), la registrazione, il controllo per il prodotto e i servizi sfruttando le funzionalità della piattaforma Windows Admin Center per soddisfare i requisiti complessi di oggi Organizzazioni IT.
- **Sviluppare utilizzando le tecnologie Web più recenti:** Crea rapidamente esperienze utente straordinarie usando tecnologie Web moderne, tra cui HTML5, CSS, angolare, TypeScript e jQuery, oltre a potenti controlli dell'interfaccia utente inclusi nell'SDK di Windows Admin Center.
- **Estendi la copertura del prodotto:** Entra a far parte del nuovo ecosistema di amministrazione di Windows con il raggiungimento della base dei clienti in rapida crescita e sfrutta il momento di avvio di Windows Server 2019 più avanti nell'anno.

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>Inizia a sviluppare con Windows Admin Center SDK

Iniziare a usare lo sviluppo con il centro di amministrazione di Windows è facile.  Il codice di esempio è disponibile per i tipi di estensione di plug-in per [strumenti](develop-tool.md), [soluzioni](develop-solution.md)e [gateway](develop-gateway-plugin.md) nella documentazione di SDK. Sarà possibile usare l'interfaccia della riga di comando di Windows Admin Center per compilare un nuovo progetto di estensione, quindi seguire le singole guide per personalizzare il progetto in base alle esigenze.

È stato reso disponibile un toolkit di [progettazione](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) di Windows Admin Center SDK che consente di simulare rapidamente le estensioni in PowerPoint usando gli stili, i controlli e i modelli di pagina dell'interfaccia di amministrazione di Windows. Prima di iniziare a scrivere il codice, vedere l'aspetto dell'estensione nell'interfaccia di amministrazione di Windows.

È disponibile anche codice di esempio ospitato in GitHub: [Strumenti di sviluppo](https://aka.ms/wacsdk) è un'estensione della soluzione di esempio contenente un'ampia raccolta di controlli che è possibile esplorare e utilizzare nella propria estensione. Strumenti di sviluppo è un'estensione completamente funzionante che può essere trasferita localmente in Windows Admin Center in modalità di sviluppo.

Vedi gli argomenti riportati di seguito per altre informazioni su SDK e su come iniziare:

- [Informazioni sul funzionamento delle estensioni](understand-extensions.md)
- [Sviluppare un'estensione](developing-extensions.md)
- [Guide](guides.md)
- [Pubblicare l'estensione](publish-extensions.md)

## <a name="partner-spotlight"></a>Partner in evidenza

Scopri l'enorme valore che i nostri partner hanno iniziato ad apportare all'ecosistema di Windows Admin Center e prova subito le loro estensioni. Altre informazioni su [come installare le estensioni](../configure/using-extensions.md) da Windows Admin Center.

### <a name="biitops"></a>BiitOps
L'estensione BiitOps changes fornisce il rilevamento delle modifiche per l'hardware, il software e le impostazioni di configurazione sulle macchine virtuali o fisiche di Windows Server. L'estensione BiitOps changes mostrerà esattamente quali sono le novità, cosa è stato modificato e cosa è stato eliminato in un singolo riquadro di vetro per tenere traccia dei problemi relativi a conformità, affidabilità e sicurezza. [Altre informazioni sull'estensione BiitOps changes](case-studies/biitops.md).

![Estensione BiitOps](../media/extensibility-overview/biitops-1.png)

### <a name="dataon"></a>DataON

L'estensione DataON deve fornire informazioni dettagliate su monitoraggio, gestione e end-to-end nell'infrastruttura iperconvergente di DataON e nei sistemi di archiviazione basati su Windows Server. L'estensione MUST aggiunge un valore univoco, ad esempio la creazione di report cronologici dei dati, il mapping del disco, gli avvisi di sistema e il servizio di chiamata a SAN, che integra il server dell'interfaccia di amministrazione di Windows e le funzionalità di gestione dell'infrastruttura iperconvergente, grazie a una soluzione semplice esperienza unificata. [Altre informazioni sull'estensione DataON MUST e sull'esperienza di sviluppo](case-studies/dataon.md).

![Estensione DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

Le estensioni ServerView Health e RAID Health di Fujitsu per l'interfaccia di amministrazione di Windows forniscono funzionalità di monitoraggio e gestione approfondite dei componenti hardware critici, ad esempio processori, memoria, alimentazione e sottosistemi di archiviazione per i server Fujitsu PRIMERGY. Usando i controlli dell'interfaccia utente e i modelli di progettazione UX di Windows Admin Center, Fujitsu ha consentito un enorme avanzamento verso la nostra visione di una comprensione end-to-end di servizi e ruoli server, del sistema operativo e della gestione dell'hardware grazie alla piattaforma Windows Admin Center. [Altre informazioni sulle estensioni Fujitsu e sull'esperienza di sviluppo](case-studies/fujitsu.md).

![Estensione Fujitsu ServerView](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

L'estensione Lenovo XClarity Integrator consente la gestione dell'hardware al livello successivo grazie all'integrazione semplificata in diverse esperienze all'interno dell'interfaccia di amministrazione di Windows. La soluzione XClarity Integrator fornisce una visualizzazione di alto livello di tutti i server Lenovo e diverse estensioni degli strumenti forniscono dettagli sull'hardware se si è connessi a un singolo server, a un cluster di failover o a un cluster iperconvergente. [Altre informazioni sull'estensione Lenovo XClarity Integrator](case-studies/lenovo.md).

![Estensione Lenovo](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

L'archiviazione pure offre soluzioni di archiviazione dei dati aziendali e di tutti i flash che offrono un'architettura incentrata sui dati per accelerare l'attività aziendale per un vantaggio competitivo. L'estensione di archiviazione pure per l'interfaccia di amministrazione di Windows offre una visualizzazione a un unico riquadro dei prodotti FlashArray puri e consente agli utenti di eseguire attività di monitoraggio, visualizzare le metriche delle prestazioni in tempo reale e gestire i volumi e gli iniziatori di archiviazione tramite un'unica interfaccia utente esperienza. [Scopri di più sulle estensioni pure e sull'esperienza di sviluppo](case-studies/purestorage.md).

![Estensione di archiviazione pure](../media/extensibility-overview/purestorage-extension.png)

### <a name="qct"></a>QCT

L'estensione QCT Management Suite è complementare all'interfaccia di amministrazione di Windows offrendo la gestione e il monitoraggio dei server fisici per i sistemi certificati QCT Azure Stack HCI. L'estensione QCT Management Suite Visualizza informazioni sull'hardware del server e fornisce un'interfaccia utente intuitiva per la sostituzione dei dischi fisici in modo efficiente, strumenti per il registro eventi hardware e S.M.A.R.T. gestione dei dischi predittivi basata su. [Altre informazioni sull'estensione QCT Management Suite](case-studies/qct.md).

![Estensione QCT](../media/extensibility-overview/qct-extension.png)

### <a name="squared-up"></a>Squared Up

Squared Up offre la migliore esperienza di monitoraggio del settore, basata su System Center Operations Manager e integrata con Azure Log Analytics, Application Insights e altre soluzioni di monitoraggio. L'[estensione Squared Up](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu) porta i dati cronologici sulle prestazioni e le topologie e le dipendenze delle applicazioni live nel contesto della gestione di server e cluster offerta da Windows Admin Center e i primi clienti hanno confermato l'enorme vantaggio di poter disporre di un'immensa quantità di dati provenienti da fonti diverse in un'unica posizione. [Altre informazioni sull'estensione Squared Up e sull'esperienza di sviluppo](case-studies/squared-up.md).

![Estensione Squared Up](../media/extensibility-overview/squaredup-extension.png)