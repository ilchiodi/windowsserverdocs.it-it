---
title: Funzionalità rimosse o pianificate per la rimozione in Windows Server 2019
description: Informazioni sulle caratteristiche e rimosse o pianificate per la rimozione a partire da Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: jasgro
ms.date: 03/29/2019
ms.localizationpriority: medium
ms.openlocfilehash: 470857616a9b36d238de031b4ccf80a68eff1e61
ms.sourcegitcommit: 971f6538e8d89af84ef50fc8aab2188bdf6f47cb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2019
ms.locfileid: "9279132"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Funzionalità rimosse o pianificate per la sostituzione di Windows Server 2019

>Si applica a: Windows Server 2019

In ogni versione di Windows Server vengono aggiunte nuove caratteristiche e funzionalità. Di tanto in tanto, vengono anche rimosse caratteristiche e funzionalità precedenti, sostituite con un'opzione migliore. Ecco i dettagli sulle caratteristiche e che sono state rimosse in Windows Server 2019.   

> [!TIP]
> - Puoi accedere in anteprima alle build di Windows Server partecipando al [programma Windows Insider](https://insider.windows.com): è un ottimo modo per sperimentare le modifiche relative alle funzionalità.
> - Se hai domande su altre versioni, Consulta le informazioni per [Windows Server, versione 1803](../get-started/windows-server-1803-removed-features.md), [Windows Server, versione 1709](../get-started/removed-features-1709.md)e [Windows Server 2016](../get-started/deprecated-features.md).

**L'elenco è soggetto a modifiche e potrebbe non includere tutte le caratteristiche o le funzionalità interessate.** 

## <a name="features-we-removed-in-this-release"></a>Funzionalità rimosse in questa versione

Ci stiamo rimuovendo le seguenti caratteristiche e funzionalità dall'immagine del prodotto installato in Windows Server 2019. Il codice o le applicazioni che dipendono da tali funzionalità non funzioneranno in questa versione, a meno che non venga utilizzato un metodo alternativo.   

|Funzionalità    |In alternativa puoi utilizzare...|
|-----------|--------------------
|Digitalizzazione aziendale, anche denominata Gestione digitalizzazione distribuita|Abbiamo stiamo la rimozione di questo digitalizzazione e la funzionalità di gestione dello scanner: non sono presenti dispositivi che supportano questa funzionalità.|
|Stampare componenti - componente facoltativo ora per le installazioni Server Core|Nelle versioni precedenti di Windows Server, i componenti di stampa sono state *disabilitate* per impostazione predefinita nell'opzione di installazione Server Core. Abbiamo modificato che in Windows Server 2016, consentendo loro per impostazione predefinita. In Windows Server 2019, tali componenti di stampa sono ancora una volta disabilitati per impostazione predefinita per Server Core. Se hai bisogno abilitare i componenti di stampa, puoi farlo eseguendo il cmdlet di **Installazione-WindowsFeature Server di stampa** .|
|[Gestore connessione Desktop remoto e Host di virtualizzazione Desktop remoto](../remote/remote-desktop-services/desktop-hosting-service.md) in un'installazione dei componenti di base del server|Nella maggior parte delle distribuzioni di Servizi Desktop remoto, il percorso di questi ruoli è condiviso con Host sessione Desktop remoto, che richiede Server con Esperienza desktop; pertanto, per garantire la coerenza con Host sessione Desktop remoto, stiamo apportando modifiche ai ruoli in questione imponendo anche per essi il requisito di Server con Esperienza desktop. Questi ruoli di servizi desktop remoto non sono più disponibili per l'uso in un' [installazione Server Core](../administration/server-core/what-is-server-core.md). Se hai bisogno per [distribuire questi ruoli nell'ambito dell'infrastruttura di Desktop remoto](../remote/remote-desktop-services/rds-deploy-infrastructure.md), è possibile [installarli su Windows Server con esperienza Desktop](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Questi ruoli sono inclusi anche nell'opzione di installazione Esperienza desktop di Windows Server 2019. |



## <a name="features-were-no-longer-developing"></a>Funzionalità di cui è cessato lo sviluppo

Ti stai non è più le funzionalità sviluppate e potranno essere rimosse con un aggiornamento futuro. Alcune sono state sostituite con altre caratteristiche o funzionalità, mentre altre sono ora disponibili da origini differenti. 

Se hai commenti e suggerimenti da condividere sulla sostituzione proposta di una di queste funzionalità, puoi utilizzare l'[app Hub di Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

|Funzionalità    |In alternativa puoi utilizzare...|
|-----------|---------------------|
|Unità di archiviazione della chiave in Hyper-V|Stiamo lavorando non è più la funzionalità di unità di archiviazione della chiave in Hyper-V. Se stai usando le macchine virtuali della generazione 1, consulta [Protezione di virtualizzazione della generazione 1 macchina virtuale](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) per informazioni sulle opzioni da ora in poi. Se stai creando nuove macchine virtuali usano le macchine virtuali della generazione 2 con i dispositivi TPM per una soluzione più sicura. |
|Console di gestione Trusted Platform Module (TPM)|Le informazioni in precedenza disponibile nella console di gestione TPM sono ora disponibile nella pagina [**sicurezza dei dispositivi**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) in [Windows Defender Security Center](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center).|
|Modalità attestazione Active Directory servizio sorveglianza host|Stiamo sviluppando non è più modalità attestazione di Active Directory servizio sorveglianza Host: invece abbiamo aggiunto una nuova modalità di attestazione dell' [attestazione della chiave host](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), che è molto più semplice e altrettanto compatibile come attestazione basata su Active Directory.  Questa nuova modalità offre funzionalità equivalenti con un'esperienza di installazione, gestione più semplice e le dipendenze delle infrastrutture un numero minore rispetto all'attestazione di Active Directory. Attestazione della chiave host non ha alcuna requisiti hardware aggiuntivi oltre la quale attestazione di Active Directory obbligatorio, in modo che tutti i sistemi esistenti rimarrà compatibili con la modalità di nuovo. Per ulteriori informazioni sulle opzioni di attestazione, vedi [Distribuisci sorvegliata host](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) .|
|Servizio OneSync|Il servizio OneSync Sincronizza i dati per le app posta, calendario e contatti. Abbiamo aggiunto un motore di sincronizzazione per l'app di Outlook che fornisce la stessa sincronizzazione.|
|Supporto delle API di compressione differenziale remoto|Supporto delle API di compressione differenziale remoto abilitati la sincronizzazione dei dati con un'origine remota con tecnologie di compressione, che la quantità di dati inviati attraverso la rete in modalità ridotta. Questo supporto non è attualmente utilizzato da qualsiasi prodotto Microsoft.|
|Estensione del commutatore filtro leggero protezione file Windows|L'estensione di switch di filtro leggero protezione file Windows consente agli sviluppatori di creare [le estensioni filtro rete semplice dei pacchetti per Hyper-V virtual switch](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering). Puoi ottenere le stesse funzionalità, crea un'estensione del filtro completa. Di conseguenza, ti verranno rimosse questa estensione in futuro.|

