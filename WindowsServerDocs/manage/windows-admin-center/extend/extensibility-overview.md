---
title: Estensioni per l'interfaccia di amministrazione di Windows
description: Estensioni per Windows Admin Center SDK (Project Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 010ab340dc71d199119f1bd51fbc22e3ad449040
ms.sourcegitcommit: 074b59341640a8ae0586d6b37df7ba256e03a0c6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/21/2020
ms.locfileid: "81650085"
---
# <a name="extensions-for-windows-admin-center"></a>Estensioni per l'interfaccia di amministrazione di Windows

>Si applica a: Windows Admin Center, Windows Admin Center Preview

L'interfaccia di amministrazione di Windows è stata creata come piattaforma estendibile per consentire a partner e sviluppatori di sfruttare le funzionalità esistenti nell'interfaccia di amministrazione di Windows, integrarsi facilmente con altri prodotti e soluzioni di amministrazione IT e fornire valore aggiunto ai clienti. Ogni soluzione e strumento nell'interfaccia di amministrazione di Windows è stato creato come estensione che usa le stesse funzionalità di estendibilità disponibili per partner e sviluppatori, in modo da poter creare strumenti avanzati esattamente come quelli disponibili nell'interfaccia di amministrazione di Windows.

Le estensioni dell'interfaccia di amministrazione di Windows sono compilate usando tecnologie Web moderne, tra cui HTML5, CSS, angolare, TypeScript e jQuery, e possono gestire i server di destinazione tramite PowerShell o WMI. È anche possibile gestire i server, i servizi o i dispositivi di destinazione su protocolli diversi, ad esempio REST, creando un plug-in del gateway dell'interfaccia di amministrazione di Windows.

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>Perché considerare la possibilità di sviluppare un'estensione per l'interfaccia di amministrazione di Windows

Questo è il valore che è possibile portare al prodotto e ai clienti sviluppando estensioni per l'interfaccia di amministrazione di Windows:

- Eseguire l' **integrazione con gli strumenti di amministrazione di Windows:** Integra i tuoi prodotti e servizi con gli strumenti di gestione di server e cluster nell'interfaccia di amministrazione di Windows e Distribuisci le esperienze di monitoraggio, gestione e risoluzione dei problemi unificate e senza problemi ai tuoi clienti.
- **Sfruttare le funzionalità di sicurezza, identità e gestione della piattaforma:** Abilita il supporto Azure Active Directory (AAD), Multi-Factor Authentication, il controllo degli accessi in base al ruolo (RBAC), la registrazione, il controllo per il prodotto e i servizi sfruttando le funzionalità della piattaforma Windows Admin Center per soddisfare i requisiti complessi delle attuali organizzazioni IT.
- **Sviluppare utilizzando le tecnologie Web più recenti:** Crea rapidamente esperienze utente straordinarie usando tecnologie Web moderne, tra cui HTML5, CSS, angolare, TypeScript e jQuery, oltre a potenti controlli dell'interfaccia utente inclusi nell'SDK di Windows Admin Center.
- **Estendi la copertura del prodotto:** Diventa parte dell'ecosistema dell'interfaccia di amministrazione di Windows con il raggiungimento della base dei clienti in espansione.

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>Inizia a sviluppare con Windows Admin Center SDK

Iniziare a usare lo sviluppo con il centro di amministrazione di Windows è facile.  Il codice di esempio è disponibile per i tipi di estensione di plug-in per [strumenti](develop-tool.md), [soluzioni](develop-solution.md)e [gateway](develop-gateway-plugin.md) nella documentazione di SDK. Sarà possibile usare l'interfaccia della riga di comando di Windows Admin Center per compilare un nuovo progetto di estensione, quindi seguire le singole guide per personalizzare il progetto in base alle esigenze.

È stato reso disponibile un toolkit di [progettazione](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) di Windows Admin Center SDK che consente di simulare rapidamente le estensioni in PowerPoint usando gli stili, i controlli e i modelli di pagina dell'interfaccia di amministrazione di Windows. Prima di iniziare a scrivere il codice, vedere l'aspetto dell'estensione nell'interfaccia di amministrazione di Windows.

È disponibile anche codice di esempio ospitato in GitHub: [strumenti di sviluppo](https://aka.ms/wacsdk) è un'estensione della soluzione di esempio contenente una raccolta completa di controlli che è possibile esplorare e usare nella propria estensione. Strumenti di sviluppo è un'estensione completamente funzionante che può essere caricata a fianco nell'interfaccia di amministrazione di Windows in modalità sviluppatore.

Per ulteriori informazioni sull'SDK e per iniziare, vedere gli argomenti seguenti:

- [Informazioni sul funzionamento delle estensioni](understand-extensions.md)
- [Sviluppare un'estensione](developing-extensions.md)
- [Guide](guides.md)
- [Pubblicare l'estensione](publish-extensions.md)

## <a name="partner-spotlight"></a>Spotlight partner

Scopri il valore straordinario che i nostri partner hanno iniziato a portare all'ecosistema dell'interfaccia di amministrazione di Windows e provano queste estensioni oggi stesso. Altre informazioni su [come installare le estensioni](../configure/using-extensions.md) dall'interfaccia di amministrazione di Windows.

### <a name="biitops"></a>BiitOps
L'estensione BiitOps changes fornisce il rilevamento delle modifiche per l'hardware, il software e le impostazioni di configurazione sulle macchine virtuali o fisiche di Windows Server. L'estensione BiitOps changes mostrerà esattamente quali sono le novità, cosa è stato modificato e cosa è stato eliminato in un singolo riquadro di vetro per tenere traccia dei problemi relativi a conformità, affidabilità e sicurezza. [Altre informazioni sull'estensione BiitOps changes](case-studies/biitops.md).

![Estensione BiitOps](../media/extensibility-overview/biitops-1.png)

### <a name="dataon"></a>DataON

L'estensione DataON deve fornire informazioni dettagliate su monitoraggio, gestione e end-to-end nell'infrastruttura iperconvergente di DataON e nei sistemi di archiviazione basati su Windows Server. L'estensione MUST aggiunge un valore univoco, ad esempio la creazione di report cronologici dei dati, il mapping del disco, gli avvisi di sistema e il servizio di chiamata a SAN, che integra il server dell'interfaccia di amministrazione di Windows e le funzionalità di gestione dell'infrastruttura iperconvergente, grazie a un'esperienza unificata e semplice. [Altre informazioni sull'estensione must di DataON e sull'esperienza di sviluppo](case-studies/dataon.md).

![DataON deve essere estensione](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

Le estensioni ServerView Health e RAID Health di Fujitsu per l'interfaccia di amministrazione di Windows forniscono funzionalità di monitoraggio e gestione approfondite dei componenti hardware critici, ad esempio processori, memoria, alimentazione e sottosistemi di archiviazione per i server Fujitsu PRIMERGY. Grazie all'utilizzo dei modelli di progettazione e dei controlli dell'interfaccia utente di Windows Admin Center, Fujitsu ha offerto un notevole passo avanti verso la visione dell'analisi end-to-end dei ruoli e dei servizi del server, del sistema operativo e della gestione dell'hardware tramite la piattaforma Windows Admin Center. [Scopri di più sulle estensioni di Fujitsu e sull'esperienza di sviluppo](case-studies/fujitsu.md).

![Estensione ServerView Fujitsu](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

L'estensione Lenovo XClarity Integrator consente la gestione dell'hardware al livello successivo grazie all'integrazione semplificata in diverse esperienze all'interno dell'interfaccia di amministrazione di Windows. La soluzione XClarity Integrator fornisce una visualizzazione di alto livello di tutti i server Lenovo e diverse estensioni degli strumenti forniscono dettagli sull'hardware se si è connessi a un singolo server, a un cluster di failover o a un cluster iperconvergente. [Altre informazioni sull'estensione Lenovo XClarity Integrator](case-studies/lenovo.md).

![Estensione Lenovo](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

L'archiviazione pure offre soluzioni di archiviazione dei dati aziendali e di tutti i flash che offrono un'architettura incentrata sui dati per accelerare l'attività aziendale per un vantaggio competitivo. L'estensione di archiviazione pure per l'interfaccia di amministrazione di Windows offre una visualizzazione a un unico riquadro dei prodotti FlashArray puri e consente agli utenti di eseguire attività di monitoraggio, visualizzare le metriche delle prestazioni in tempo reale e gestire i volumi e gli iniziatori di archiviazione tramite un'unica esperienza dell'interfaccia utente. [Scopri di più sulle estensioni pure e sull'esperienza di sviluppo](case-studies/purestorage.md).

![Estensione di archiviazione pure](../media/extensibility-overview/purestorage-extension.png)

### <a name="qct"></a>QCT

L'estensione QCT Management Suite è complementare all'interfaccia di amministrazione di Windows offrendo la gestione e il monitoraggio dei server fisici per i sistemi certificati QCT Azure Stack HCI. L'estensione QCT Management Suite Visualizza informazioni sull'hardware del server e fornisce un'interfaccia utente intuitiva per la sostituzione dei dischi fisici in modo efficiente, strumenti per il registro eventi hardware e S.M.A.R.T. gestione dei dischi predittivi basata su. [Altre informazioni sull'estensione QCT Management Suite](case-studies/qct.md).

![Estensione QCT](../media/extensibility-overview/qct-extension.png)