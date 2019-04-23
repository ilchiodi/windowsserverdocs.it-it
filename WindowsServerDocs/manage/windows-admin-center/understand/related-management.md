---
title: Windows Admin Center correlati soluzioni di gestione
description: Come Windows Admin Center vengono confrontate con e si integra con monitoraggio e gestione delle soluzioni o prodotti Microsoft (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 385a066cb828f58d698c2ca47e0553e996a77733
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847322"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center e soluzioni di gestione correlate di Microsoft

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

[Windows Admin Center](windows-admin-center.md) è l'evoluzione di tradizionale server nella finestra Strumenti di gestione per le situazioni in cui si sarebbe potuto utilizzare Desktop remoto (RDP) per connettersi a un server per la risoluzione dei problemi o la configurazione. Non è destinato a sostituire altri soluzioni esistenti di gestione Microsoft; piuttosto si integra con queste soluzioni, come descritto di seguito.

## <a name="remote-server-administration-tools-rsat"></a>Strumenti di amministrazione remota del server

[Strumenti di amministrazione remota Server (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) è una raccolta di strumenti dell'interfaccia utente e PowerShell per gestire facoltativi ruoli e funzionalità in Windows Server. Amministrazione remota del server presenta molte funzionalità che non dispone di Windows Admin Center. Si potrà aggiungere alcuni degli strumenti più usati in amministrazione remota del server per Windows Admin Center in futuro. Qualsiasi nuovo ruolo del Server di Windows o una funzionalità che richiede un'interfaccia utente grafica per la gestione sarà in Windows Admin Center.

## <a name="intune"></a>Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) è un servizio di gestione di mobilità aziendale basata sul cloud che consente di gestire i dispositivi iOS, Android, Windows e macOS, basati su un set di criteri. Intune è incentrata su come abilitare la protezione di informazioni aziendali tramite il controllo come la propria forza lavoro accede e condivide le informazioni. Windows Admin Center, invece, non è basato sui criteri, ma consente la gestione di ad hoc dei sistemi Windows 10 e Windows Server usando PowerShell e WMI remoti su WinRM.

## <a name="azure-stack"></a>Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) è una piattaforma di cloud ibrido che ti permette di offrire servizi di Azure dal tuo data center. Azure Stack viene gestito usando PowerShell o il portale dell'amministratore, che è simile al portale di Azure tradizionale usato per accedere e gestire servizi di Azure tradizionali. Windows Admin Center non è destinato a gestire l'infrastruttura di Azure Stack, ma è possibile usarlo per [gestire le macchine virtuali IaaS di Azure](../configure/manage-azure-vms.md) (in esecuzione Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012) o risolverne i problemi server fisico singolo distribuito nell'ambiente Azure Stack.

## <a name="system-center"></a>System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) è una soluzione di gestione locale data center per la distribuzione, configurazione, gestione, monitoraggio l'intero data center. System Center consente di visualizzare lo stato di tutti i sistemi nell'ambiente in uso, mentre Windows Admin Center ti permette di eseguire il drill-in un server specifico per gestire o risolve il problema con gli strumenti più granulari.

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Reinventato "in-box" piattaforma e strumenti** | **Gestione dei Data Center e monitoraggio** |
| Incluso con licenza di Windows Server – **senza alcun costo aggiuntivo**, proprio come MMC e altri strumenti integrati in tradizionale | **Completa** suite di soluzioni per il valore aggiunto nell'ambiente e le piattaforme |
| **Lightweight**, basata su browser con gestione remota delle istanze di Windows Server, **ovunque**; alternativo per la connessione RDP | Gestisci e monitora **eterogenei** systems **su larga scala**, tra cui Linux, VMware e Hyper-V |
|**Deep** a server singolo & singolo cluster drill-down per la risoluzione dei problemi, configurazione e manutenzione|Infrastruttura di provisioning; automazione e Self-Service;  monitoraggio dell'infrastruttura e del carico di lavoro **breadth**|
|Con ottimizzazione per la gestione di **singoli** 2 a 4 nodi **uomo** cluster, l'integrazione di Hyper-V, spazi di archiviazione diretta e SDN|Distribuire e gestire Hyper-V, i cluster di Windows Server **scalabilità datacenter** dalla **bare metal** con SCVMM|
|**Il monitoraggio sul uomo** archivia la cronologia di sola lettura; il servizio integrità di cluster. Piattaforma estendibile per il 1 ° e 3rd party **delle estensioni di strumenti di amministrazione**|**Extensible** & **monitoraggio scalabile** piattaforma in SCOM con avvisi, notifiche, il carico di lavoro di terze parti monitoraggio; SQL per la cronologia|
|Bridge più semplice per **ibrida**; eseguire l'onboarding e usare un'ampia gamma di servizi di Azure per la protezione dei dati, replica, gli aggiornamenti e altro ancora|**Predefiniti** protezione dei dati, replica, gli aggiornamenti (Data Protection Manager/VMM/SCCM). Integrazione ibrida con Log Analitica e mapping dei servizi|
|**Funzionalità della piattaforma si illumina** di Windows Server: Informazioni dettagliate di sistema servizio di migrazione, Replica archiviazione, archiviazione e così via.|**Altre piattaforme**: Automazione in Orchestrator o SMA. Integrazioni con SCSM & altri strumenti di gestione del servizio|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>Ognuno offre il valore di destinazione in modo indipendente; **meglio insieme** con funzionalità complementare.
