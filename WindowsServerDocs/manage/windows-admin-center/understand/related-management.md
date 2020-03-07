---
title: Soluzioni di gestione correlate di Windows Admin Center
description: Confronto e integrazione di Windows Admin Center con altre soluzioni/prodotti Microsoft di gestione e monitoraggio (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d681e5007cd3ae3c14de774df0bc85abc23b51d7
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371688"
---
# <a name="windows-admin-center-and-related-management-solutions-from-microsoft"></a>Windows Admin Center e soluzioni di gestione correlate di Microsoft

>Si applica a: Windows Admin Center, Windows Admin Center Preview

[Windows Admin Center](windows-admin-center.md) è l'evoluzione dei tradizionali strumenti di gestione predefiniti dei server per le situazioni in cui è possibile usare un Desktop remoto (RDP) per connettersi a un server per la risoluzione dei problemi o la configurazione. Non è destinato a sostituire altre soluzioni di gestione Microsoft esistenti, piuttosto integra queste soluzioni, come descritto di seguito.

## <a name="remote-server-administration-tools-rsat"></a>Strumenti di amministrazione remota del server

[Strumenti di amministrazione remota del server (RSAT)](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools) è una raccolta di strumenti PowerShell e interfaccia utente grafica per gestire i ruoli e le funzionalità facoltative in Windows Server. Strumenti di amministrazione remota del server offre molte funzionalità non presenti in Windows Admin Center. In futuro si potranno aggiungere in Windows Admin Center alcuni degli strumenti più usati di Strumenti di amministrazione remota del server. Qualsiasi nuova funzionalità o ruolo di Windows Server la cui gestione richieda un'interfaccia utente grafica si troverà in Windows Admin Center.

## <a name="intune"></a>Intune

[Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) è un servizio di gestione della mobilità aziendale basato sul cloud che consente di gestire i dispositivi iOS, Android, Windows e macOS, in base a un set di criteri. Intune consente di proteggere le informazioni aziendali controllando il modo in cui la forza lavoro accede e condivide le informazioni. Windows Admin Center, invece, non si basa sui criteri ma consente una gestione ad hoc dei sistemi Windows 10 e Windows Server usando PowerShell e WMI remoti su WinRM.

## <a name="azure-stack"></a>Azure Stack

[Azure Stack](https://azure.microsoft.com/overview/azure-stack/) è una piattaforma cloud ibrida che consente di offrire servizi di Azure dal proprio data center. Azure Stack viene gestito usando PowerShell o il portale dell'amministratore, simile al portale di Azure tradizionale usato per accedere e gestire i servizi di Azure tradizionali. Windows Admin Center non è destinato a gestire l'infrastruttura di Azure Stack, ma è possibile usarlo per la [gestione di macchine virtuali IaaS di Azure](../azure/manage-azure-vms.md) (che eseguono Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012) o la risoluzione di problemi relativi a singoli server fisici distribuiti nell'ambiente Azure Stack.

## <a name="system-center"></a>System Center

[System Center](https://www.microsoft.com/cloud-platform/system-center) è una soluzione di gestione di data center locale per la distribuzione, la configurazione, la gestione e il monitoraggio dell'intero data center. System Center consente di visualizzare lo stato di tutti i sistemi dell'ambiente, mentre Windows Admin Center consente di eseguire il drill-down in un server specifico per gestire o risolve il problema con strumenti più granulari.

| Windows Admin Center                 | System Center                      |
|--------------------------------------|------------------------------------|
| **Riprogettazione di piattaforme e strumenti "predefiniti"** | **Monitoraggio e gestione di data center** |
| Incluso con la licenza di Windows Server, **senza costi aggiuntivi**, proprio come MMC e altri strumenti tradizionali integrati | Suite **completa** di soluzioni per un valore aggiunto all'ambiente e alle piattaforme |
| Gestione remota **leggera** in base al browser delle istanze di Windows Server, **ovunque**, un'alternativa a RDP | È possibile gestire e monitorare sistemi **eterogenei** e **su larga scala**, tra cui Hyper-V, VMware e Linux |
|Esecuzione di un drill-down **approfondito** su singolo server e singolo cluster per la risoluzione di problemi, configurazione e manutenzione|Provisioning dell'infrastruttura, automazione e self-service, monitoraggio dell'infrastruttura e del carico di lavoro **breadth**|
|Gestione ottimizzata di **singoli** cluster **HCI** a 2-4 nodi, che integrano Hyper-V, spazi di archiviazione diretta e SDN|È possibile distribuire e gestire Hyper-V, cluster Windows Server su **scala di data center** da **bare metal** con SCVMM|
|Soltanto **monitoraggio sul HCI**, il servizio integrità del cluster archivia la cronologia. Piattaforma estendibile per la prima e la terza parte delle **estensioni dello strumento di amministrazione**|Piattaforma **estendibile** e  & **monitoraggio scalabile** in SCOM con avvisi, notifiche, monitoraggio del carico di lavoro di terze parti. SQL con la cronologia|
|La via più semplice verso l'**ibrido**. Esegue l'onboarding e usa un'ampia gamma di servizi di Azure per la protezione dei dati, la replica, gli aggiornamenti e altro ancora|**Integrazione** di protezione dati, replica e aggiornamenti (DPM/VMM/SCCM). Integrazione ibrida con Log Analytics e Service Map|
|**Accende le funzionalità della piattaforma** di Windows Server: servizio migrazione archiviazione, replica archiviazione, informazioni dettagliate sul sistema e così via.|**Piattaforme aggiuntive**: automazione nell'agente di orchestrazione/SMA. Integrazioni con SCSM & altri strumenti di gestione dei servizi|

#### <a name="each-delivers-targeted-value-independently-better-together-with-complementary-capabilities"></a>Ognuno di essi offre un valore specifico in modo indipendente. Se **usati insieme** consentono funzionalità complementari.
