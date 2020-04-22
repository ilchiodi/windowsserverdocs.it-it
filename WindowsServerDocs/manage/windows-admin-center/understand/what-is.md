---
title: Che cos'è Windows Admin Center?
description: Informazioni su Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: cb4e3ab2bf98a0c2d51483642fe5388e468dbbb4
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/13/2020
ms.locfileid: "81269268"
---
# <a name="what-is-windows-admin-center"></a>Che cos'è Windows Admin Center?

> Si applica a: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center è un nuovo strumento di gestione basato su browser e distribuito localmente, che consente di gestire i server Windows senza dipendenze cloud o Azure. Windows Admin Center offre il controllo completo su tutti gli aspetti dell'infrastruttura server ed è particolarmente utile per la gestione dei server su reti private non connesse a Internet.

Windows Admin Center è l'evoluzione moderna degli strumenti di gestione "inclusi nel prodotto", ad esempio Server Manager e MMC. Si integra con System Center, ma non lo sostituisce.

![](../media/wac-complements.png)

## <a name="how-does-windows-admin-center-work"></a>Come funziona Windows Admin Center?

Windows Admin Center viene eseguito in un Web browser e gestisce Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 10 e altri sistemi tramite il **gateway di Windows Admin Center** installato in Windows Server o in Windows 10 aggiunto a un dominio. Il gateway gestisce i server tramite la sessione remota di PowerShell e WMI su WinRM. Il gateway è incluso in Windows Admin Center in un unico pacchetto con estensione msi leggero che è possibile [scaricare](https://aka.ms/windowsadmincenter).

Il gateway di Windows Admin Center, quando viene pubblicato su DNS e ottiene l'accesso tramite i firewall aziendali corrispondenti, ti consente di connetterti e gestire i server in modo sicuro da qualsiasi luogo con Microsoft Edge o Google Chrome.

![](../media/architecture.png)

## <a name="learn-how-windows-admin-center-improves-your-management-environment"></a>Scopri come Windows Admin Center può migliorare la gestione dell'ambiente

### <a name="familiar-functionality"></a>**Funzionalità familiari**

Windows Admin Center è l'evoluzione di piattaforme di gestione consolidate e di lunga data come Microsoft Management Console (MMC), concepite fin dall'inizio per il modo in cui i sistemi sono creati e gestiti oggi. Windows Admin Center contiene molti degli strumenti familiari attualmente in uso per gestire server e client Windows.

### <a name="easy-to-install-and-use"></a>**Facilità di installazione e uso**

[Viene installato](../deploy/install.md) in un computer Windows 10 e la gestione inizia dopo pochi minuti oppure viene installato in un server Windows 2016, fungendo da gateway per consentire all'intera organizzazione di gestire i computer dal Web browser.

### <a name="complements-existing-solutions"></a>**Complementarietà con le soluzioni esistenti**

Windows Admin Center funziona con soluzioni come System Center e gestione e sicurezza di Azure, aggiungendo alle capacità di queste soluzioni la possibilità di eseguire attività di gestione dettagliate, da un solo computer.

### <a name="manage-from-anywhere"></a>**Gestione da qualsiasi luogo**

Puoi pubblicare il server gateway di Windows Admin Center su una rete Internet pubblica e quindi connetterti e gestire i tuoi server da qualsiasi luogo, in maniera assolutamente sicura.

### <a name="enhanced-security-for-your-management-platform"></a>**Protezione avanzata per la piattaforma di gestione**

Windows Admin Center include molti miglioramenti che rendono [più sicura](../plan/user-access-options.md) la tua piattaforma di gestione. Il controllo degli accessi in base al ruolo ti consente di scegliere gli amministratori che hanno accesso alle varie funzioni di gestione. Le opzioni di autenticazione del gateway includono gruppi locali, Active Directory basata su dominio locale e Azure Active Directory basata sul cloud.  Puoi [ottenere qui informazioni dettagliate](../use/logging.md) sulle azioni di gestione eseguite nell'ambiente in uso.

### <a name="azure-integration"></a>**Integrazione di Azure**

Windows Admin Center ha molti punti di [integrazione con i servizi di Azure](../plan/azure-integration-options.md), compresi Azure Activity Directory, Azure Backup, Azure Site Recovery e altro ancora.

### <a name="manage-hyper-converged-clusters"></a>**Gestione dei cluster iperconvergenti**

Windows Admin Center offre un'esperienza ottimale per la [gestione dei cluster iperconvergenti](../use/manage-hyper-converged.md), inclusi componenti virtualizzati di calcolo, archiviazione e gestione di rete.

### <a name="extensibility"></a>**Estensibilità**

Windows Admin Center è stato progettato fin dall'inizio per l'estensibilità, con la possibilità per gli sviluppatori Microsoft e di terze parti di creare strumenti e soluzioni in più rispetto alle offerte correnti. Microsoft offre un [SDK](../extend/extensibility-overview.md) che consente agli sviluppatori di creare i propri strumenti per Windows Admin Center.

> [!Tip]
> Pronto per installare Windows Admin Center? [Scarica subito](https://aka.ms/windowsadmincenter)
