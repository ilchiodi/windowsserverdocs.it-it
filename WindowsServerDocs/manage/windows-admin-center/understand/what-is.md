---
title: Che cos'è Windows Admin Center
description: Che cos'è Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d374704768d06aaeb7ee6bc9c984cc78d49cb4b0
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080908"
---
# Che cos'è Windows Admin Center?

>Si applica a: Windows Admin Center, Anteprima di Windows Admin Center

Windows Admin Center è un nuovo strumento di gestione basato su browser, distribuito localmente che ti consente di gestire i server Windows senza dipendenze cloud o Azure. Windows Admin Center ti offre il controllo completo su tutti gli aspetti dell'infrastruttura server ed è particolarmente utile per la gestione dei server su reti private non connesse a Internet.

Windows Admin Center è l'evoluzione moderna degli strumenti di gestione "inclusi", ad esempio Server Manager e MMC. Può integrarlo System Center, non è una sostituzione.

![](../media/wac-complements.png)

## Come funziona Windows Admin Center?

Windows Admin Center viene eseguito in un Web browser e gestisce Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 10 e altro tramite il **gateway di Windows Admin Center** installato su Windows Server 2016 o Windows 10. Il gateway gestisce i server tramite la sessione remota di PowerShell e WMI su WinRM. Il gateway è incluso in Windows Admin Center in un unico pacchetto MSI leggero che è possibile [scaricare](https://aka.ms/windowsadmincenter).

Il gateway Windows Admin Center, quando viene pubblicato su DNS e ottiene l'accesso tramite i firewall aziendali corrispondenti, ti consente di connetterti e gestire i server in modo sicuro da qualsiasi luogo con Microsoft Edge o Google Chrome.

![](../media/architecture.png)

## Scopri come Windows Admin Center può migliorare la gestione del tuo ambiente

### **Funzionalità familiare**

Windows Admin Center è l'evoluzione di piattaforme di gestione consolidate e di lunga data come Microsoft Management Console (MMC), concepite fin dall'inizio per il modo in cui i sistemi sono creati e gestiti oggi. Windows Admin Center contiene molti degli strumenti familiari attualmente in uso per gestire server e client Windows.

### **Facilità di installazione e utilizzo**

[Viene installato](../deploy/install.md) su un computer Windows 10 e la gestione inizia dopo pochi minuti oppure viene installato su un server Windows 2016 fungendo da gateway per consentire all'intera organizzazione di gestire i computer dal Web browser.

### **Complementare alle soluzioni esistenti** 

Windows Admin Center funziona con soluzioni come System Center e Azure gestione e sicurezza, aggiungendo alle loro capacità di eseguire dettagliati, le attività di gestione solo computer.

### **Gestione da qualsiasi luogo**

Pubblica il server gateway Windows Admin Center su una rete Internet pubblica, puoi connetterti e gestire i tuoi server da qualsiasi luogo, in maniera assolutamente sicura.

### **Protezione avanzata per la piattaforma di gestione**

Windows Admin Center include molti miglioramenti che rendono la tua piattaforma di gestione [più sicura](../plan/user-access-options.md). Il controllo degli accessi basato sui ruoli ti consente di scegliere gli amministratori che hanno accesso alle varie funzioni di gestione. Le opzioni di autenticazione del gateway includono gruppi locali, Active Directory basata su dominio locale e Azure Active Directory basata su cloud.  Inoltre, [acquisisci i dettagli](../use/logging.md) delle azioni di gestione eseguite nell'ambiente in uso.

### **Integrazione di Azure**

Windows Admin Center ha molti punti di [integrazione con servizi di Azure](../plan/azure-integration-options.md), inclusi Azure Active Directory, Azure Backup, Azure Site Recovery e altro ancora.

### **Gestione dei cluster iper-convergenti**

Windows Admin Center offre la migliore esperienza per la [gestione dei cluster iper-convergenti](../use/manage-hyper-converged.md), inclusi componenti di calcolo, storage e networking virtualizzati.

### **Estendibilità**

Windows Admin Center è stato progettato fin dall'inizio per l'estensibilità, con la possibilità per gli sviluppatori Microsoft e di terze parti di creare strumenti e soluzioni oltre le offerte correnti. Microsoft offre un [SDK](../extend/extensibility-overview.md) che consente agli sviluppatori di creare i propri strumenti per Windows Admin Center.

> [!Tip]
> Pronto per installare Windows Admin Center? [Scarica subito](https://aka.ms/windowsadmincenter)
