---
title: Windows Admin Center correlati a soluzioni di gestione
description: Come Windows Admin Center confronta con e integra monitoraggio e gestione soluzioni/prodotti Microsoft (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7bf0e32b1156fe361c79ac4ccd0e3536df767e2
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296713"
---
# Windows Admin Center e le soluzioni di gestione correlati da Microsoft

>Si applica a: Windows Admin Center, Windows Admin Center Preview

[Windows Admin Center](windows-admin-center.md) è l'evoluzione degli strumenti di gestione tradizionali server interni per le situazioni in cui potresti hai usato Desktop remoto per connettersi a un server per la risoluzione dei problemi o la configurazione. Non è destinato a sostituire altre soluzioni Microsoft di gestione esistenti; invece, può integrarlo queste soluzioni, come descritto di seguito.

## Strumenti di amministrazione remota del server

[Strumenti di amministrazione remota Server (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) è una raccolta di strumenti GUI e PowerShell per gestire i ruoli facoltativi e funzionalità in Windows Server. RSAT ha numerose funzionalità che non dispone di Windows Admin Center. Potremmo aggiungiamo alcuni degli strumenti di uso più comune in RSAT di Windows Admin Center in futuro. Qualsiasi nuovo ruolo del Server di Windows o una funzionalità che richiede un'interfaccia utente grafica per la gestione sarà in Windows Admin Center.

## Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) è un servizio di gestione di mobilità aziendale basata sul cloud che consente di gestire i dispositivi iOS, Android, Windows e macOS, in base a un set di criteri. Intune è incentrata sulla possibilità di proteggere le informazioni aziendali controllando come i dipendenti accede e condivisioni di informazioni. Al contrario, Windows Admin Center non è basata sui criteri, ma consente la gestione di ad-hoc dei sistemi Windows 10 e Windows Server, usando remota di PowerShell e WMI su WinRM.

## Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) è una piattaforma cloud ibrido che ti consente di distribuire servizi di Azure dal centro per i dati. Azure Stack viene gestito tramite PowerShell o il portale di amministrazione, che è simile al portale di Azure tradizionale utilizzato per accedere e gestione tradizionale dei servizi di Azure. Windows Admin Center non è progettato per gestire l'infrastruttura di Azure Stack, ma puoi usarlo per [gestire le macchine virtuali IaaS di Azure](../azure/manage-azure-vms.md) (in esecuzione Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012) o risolvere i problemi di singoli fisico server distribuiti nell'ambiente Azure Stack.

## System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) è una soluzione di gestione locale centro dati per la distribuzione, configurazione, gestione, monitoraggio del centro per i dati intero. System Center consente di visualizzare lo stato di tutti i sistemi nell'ambiente, mentre Windows Admin Center consente di drill-down uno specifico server per gestire o risolvere i problemi relativi con gli strumenti più granulari.

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Strumenti & reinventato piattaforma "inclusi"** | **Monitoraggio & gestione datacenter** |
| Incluso con licenza di Windows Server- **alcun costo aggiuntivo**, proprio come MMC e altri strumenti integrati tradizionale | Suite **completa** di soluzioni per valore aggiuntiva tra l'ambiente e piattaforme |
| **Lightweight**, basata su browser gestione remota di istanze di Windows Server, **in qualsiasi punto**; alternativa a RDP | Gestire & monitor **eterogenei** sistemi **con una scala**, tra cui Hyper-V e VMware Linux |
|**Deep** & server singolo singolo cluster il drill-down per la risoluzione dei problemi, manutenzione & configurazione|Infrastruttura di provisioning; automazione e Self-Service;  carico di lavoro monitoraggio **funzionavano correttamente** e infrastruttura|
|Gestione ottimizzata di **singoli** 2-4 cluster **HCI** nodo, integrazione di Hyper-V, spazi di archiviazione diretta e SDN|Distribuire & gestione di Hyper-V, cluster di Windows Server **datacenter** scala da **bare metal** di SCVMM|
|**Il monitoraggio su HCI** di sola lettura. servizio di integrità del cluster storico. Piattaforma estendibile per parti 1 e 3 le **estensioni dello strumento di amministrazione**|**Extensible** & **scalabile monitoraggio** della piattaforma in SCOM, con gli avvisi, le notifiche, carico di lavoro di terze parti monitoraggio; SQL per la cronologia|
|Bridge più semplice per **ibrida**; eseguire l'onboarding e usare un'ampia gamma di servizi di Azure per la protezione dei dati, la replica, gli aggiornamenti e altro ancora|Protezione dei dati **incorporata** , replica, gli aggiornamenti (DPM/VMM/SCCM). Integrazione ibrida con Log Analitica e mapping servizio|
|**Attivi le funzionalità della piattaforma** di Windows Server: il servizio di migrazione di archiviazione, Replica di archiviazione, informazioni dettagliate di sistema e così via.|**Altre piattaforme**: automazione in Orchestrator/SMA. Integrazione con SCSM & altri strumenti di gestione del servizio|

#### Ogni offre il valore di destinazione in modo indipendente; **meglio insieme** a funzionalità complementare.
