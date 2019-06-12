---
title: Che cos'è Windows Admin Center
description: Che cos'è Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: 99f1a9a32ef69ba8322b2dba902003f8a750a4d2
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811650"
---
# <a name="what-is-windows-admin-center"></a>Che cos'è Windows Admin Center?

> Si applica a: Windows Admin Center, Windows Admin Center anteprima

Windows Admin Center è un nuovo strumento di gestione basato su browser, distribuito localmente che ti consente di gestire i server Windows senza dipendenze cloud o Azure. Windows Admin Center ti offre il controllo completo su tutti gli aspetti dell'infrastruttura server ed è particolarmente utile per la gestione dei server su reti private non connesse a Internet.

Windows Admin Center è l'evoluzione moderna degli strumenti di gestione "inclusi", ad esempio Server Manager e MMC. Si integra con System Center: non è una sostituzione.

![](../media/wac-complements.png)

## <a name="how-does-windows-admin-center-work"></a>Come funziona Windows Admin Center?

Windows Admin Center viene eseguito in un web browser e gestisce 2019 Server Windows, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 10 e così via, tramite il **gateway Windows Admin Center** installato in Windows Server o Windows 10. Il gateway gestisce i server tramite la sessione remota di PowerShell e WMI su WinRM. Il gateway è incluso in Windows Admin Center in un unico pacchetto MSI leggero che è possibile [scaricare](https://aka.ms/windowsadmincenter).

Il gateway Windows Admin Center, quando viene pubblicato su DNS e ottiene l'accesso tramite i firewall aziendali corrispondenti, ti consente di connetterti e gestire i server in modo sicuro da qualsiasi luogo con Microsoft Edge o Google Chrome.

![](../media/architecture.png)

## <a name="learn-how-windows-admin-center-improves-your-management-environment"></a>Scopri come Windows Admin Center può migliorare la gestione del tuo ambiente

### <a name="familiar-functionality"></a>**Funzionalità familiari**

Windows Admin Center è l'evoluzione di piattaforme di gestione consolidate e di lunga data come Microsoft Management Console (MMC), concepite fin dall'inizio per il modo in cui i sistemi sono creati e gestiti oggi. Windows Admin Center contiene molti degli strumenti familiari attualmente in uso per gestire server e client Windows.

### <a name="easy-to-install-and-use"></a>**Facile da installare e usare**

[Viene installato](../deploy/install.md) su un computer Windows 10 e la gestione inizia dopo pochi minuti oppure viene installato su un server Windows 2016 fungendo da gateway per consentire all'intera organizzazione di gestire i computer dal Web browser.

### <a name="complements-existing-solutions"></a>**Integra le soluzioni esistenti**

Windows Admin Center funziona con soluzioni quali gestione di System Center e Azure e la sicurezza, aggiungere le relative funzionalità per eseguire dettagliato, le attività di gestione singolo computer.

### <a name="manage-from-anywhere"></a>**Gestire ovunque**

Pubblica il server gateway Windows Admin Center su una rete Internet pubblica, puoi connetterti e gestire i tuoi server da qualsiasi luogo, in maniera assolutamente sicura.

### <a name="enhanced-security-for-your-management-platform"></a>**Sicurezza avanzata per la piattaforma di gestione**

Windows Admin Center include molti miglioramenti che rendono la tua piattaforma di gestione [più sicura](../plan/user-access-options.md). Il controllo degli accessi basato sui ruoli ti consente di scegliere gli amministratori che hanno accesso alle varie funzioni di gestione. Le opzioni di autenticazione del gateway includono gruppi locali, Active Directory basata su dominio locale e Azure Active Directory basata su cloud.  Inoltre, [acquisisci i dettagli](../use/logging.md) delle azioni di gestione eseguite nell'ambiente in uso.

### <a name="azure-integration"></a>**Integrazione di Azure**

Numero di punti di dispone di Windows Admin Center [integrazione con servizi di Azure](../plan/azure-integration-options.md), tra cui Azure Active Directory, Backup di Azure, Azure Site Recovery e altro ancora.

### <a name="manage-hyper-converged-clusters"></a>**Gestire i cluster iperconvergenti**

Windows Admin Center offre la migliore esperienza per la [gestione dei cluster iper-convergenti](../use/manage-hyper-converged.md), inclusi componenti di calcolo, storage e networking virtualizzati.

### <a name="extensibility"></a>**Estendibilità**

Windows Admin Center è stato progettato fin dall'inizio per l'estensibilità, con la possibilità per gli sviluppatori Microsoft e di terze parti di creare strumenti e soluzioni oltre le offerte correnti. Microsoft offre un [SDK](../extend/extensibility-overview.md) che consente agli sviluppatori di creare i propri strumenti per Windows Admin Center.

> [!Tip]
> Pronto per installare Windows Admin Center? [Scarica subito](https://aka.ms/windowsadmincenter)
