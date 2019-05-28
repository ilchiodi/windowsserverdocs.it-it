---
title: Funzionalità rimosse o pianificata la rimozione in Windows Server 2019
description: Scopri le caratteristiche e funzionalità rimosse o pianificata la rimozione a partire da Windows Server 2019.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: jasongerend
ms.author: jgerend
manager: jasgro
ms.date: 05/21/2019
ms.localizationpriority: medium
ms.openlocfilehash: 820dfed8a0a58d3ccc64023325c373b761461ba8
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/27/2019
ms.locfileid: "65976517"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Funzionalità rimosse o pianificata la sostituzione di avvio di Windows Server 2019

>Si applica a: Windows Server 2019

In ogni versione di Windows Server vengono aggiunte nuove caratteristiche e funzionalità. Di tanto in tanto, vengono anche rimosse caratteristiche e funzionalità precedenti, sostituite con un'opzione migliore. Ecco i dettagli sulle funzionalità e funzionalità che è stata rimossa in Windows Server 2019.

> [!TIP]
> - Puoi accedere in anteprima alle build di Windows Server partecipando al [programma Windows Insider](https://insider.windows.com): è un ottimo modo per sperimentare le modifiche relative alle funzionalità.
> - Se hai domande su altre versioni, Leggere le informazioni per [novità in Windows Server](../get-started/whats-new-in-windows-server.md).

**L'elenco è soggetto a modifiche e potrebbe non includere tutte le funzionalità interessate o funzionalità.** 

## <a name="features-we-removed-in-this-release"></a>Funzionalità rimosse in questa versione

Si sta rimuovendo le seguenti caratteristiche e funzionalità dall'immagine del prodotto installato in Windows Server 2019. Il codice o le applicazioni che dipendono da tali funzionalità non funzioneranno in questa versione, a meno che non venga utilizzato un metodo alternativo.

|Funzionalità    |In alternativa puoi utilizzare...|
|-----------|--------------------
|Digitalizzazione aziendale, anche denominata Gestione digitalizzazione distribuita|È corso la rimozione di questa analisi sicura e funzionalità di gestione dello scanner: non sono presenti dispositivi che supportano questa funzionalità.|
|Stampare i componenti - componente facoltativo ora per le installazioni Server Core|Nelle versioni precedenti di Windows Server, i componenti di stampa erano *disabilitato* per impostazione predefinita nell'opzione di installazione Server Core. È stato modificato, che in Windows Server 2016, consentendo per impostazione predefinita. In Windows Server 2019, tali componenti di stampa sono ancora una volta disabilitati per impostazione predefinita per Server Core. Se è necessario abilitare i componenti di stampa, è possibile farlo eseguendo il **Server di stampa Install-WindowsFeature** cmdlet.|
|[Gestore connessione Desktop remoto e Host di virtualizzazione Desktop remoto](../remote/remote-desktop-services/desktop-hosting-service.md) in un'installazione dei componenti di base del server|Nella maggior parte delle distribuzioni di Servizi Desktop remoto, il percorso di questi ruoli è condiviso con Host sessione Desktop remoto, che richiede Server con Esperienza desktop; pertanto, per garantire la coerenza con Host sessione Desktop remoto, stiamo apportando modifiche ai ruoli in questione imponendo anche per essi il requisito di Server con Esperienza desktop. Questi ruoli di servizi desktop remoto non sono più disponibili per l'uso in un [installazione Server Core](../administration/server-core/what-is-server-core.md). Se devi [distribuire questi ruoli come parte dell'infrastruttura di Desktop remoto](../remote/remote-desktop-services/rds-deploy-infrastructure.md), è possibile [installarle in Windows Server con esperienza Desktop](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Questi ruoli sono inclusi anche nell'opzione di installazione Esperienza desktop di Windows Server 2019. |

## <a name="features-were-no-longer-developing"></a>Funzionalità di cui è cessato lo sviluppo

Si sviluppano queste funzionalità non è più attivamente e possibile rimuoverli da un aggiornamento futuro. Alcune sono state sostituite con altre caratteristiche o funzionalità, mentre altre sono ora disponibili da origini differenti. 

Se hai commenti e suggerimenti da condividere sulla sostituzione proposta di una di queste funzionalità, puoi utilizzare l'[app Hub di Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

| Funzionalità   | In alternativa puoi utilizzare... |
|-----------|---------------------|
| Unità di archiviazione della chiave in Hyper-V|Non è più lavoriamo sulla funzionalità di unità di archiviazione della chiave in Hyper-V. Se si usano macchine virtuali di generazione 1, consultare [sicurezza di virtualizzazione della macchina virtuale di generazione 1](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) per informazioni sulle opzioni in futuro. Se si creano nuove macchine virtuali usano macchine virtuali di generazione 2 con i dispositivi TPM per una soluzione più sicura. |
| Console di gestione Trusted Platform Module (TPM)|Le informazioni disponibili in precedenza nella console di gestione TPM sono ora disponibile nel [ **sicurezza del dispositivo** ](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) nella pagina il [Windows Defender Security Center](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center). |
| Modalità di attestazione di Active Directory del servizio sorveglianza host|Stiamo sviluppando non è più modalità di attestazione di Active Directory del servizio sorveglianza Host, invece è stata aggiunta una nuova modalità di attestazione [ospitare l'attestazione chiave](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), che è molto più semplice e ugualmente come compatibile come basata su Active Directory attestazione.  Questa nuova modalità fornisce una funzionalità equivalente con un'esperienza di installazione, gestione più semplice e meno dipendenze dell'infrastruttura di attestazione dell'Active Directory. Attestazione chiave host non esistono requisiti hardware aggiuntivi oltre l'attestazione quali Active Directory necessari, in modo che tutti i sistemi esistenti rimarranno compatibili con la nuova modalità. Visualizzare [distribuire host sorvegliati](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) per altre informazioni sulle opzioni di attestazione. |
| Servizio OneSync|Il servizio OneSync Sincronizza i dati per le app di posta elettronica, calendario e le persone. È stato aggiunto un motore di sincronizzazione per l'app Outlook che fornisce la sincronizzazione stesso. |
| Supporto dell'API di compressione differenziale remoto|Supporto dell'API di compressione differenziale remoto è abilitata la sincronizzazione dei dati con un'origine remota usando tecnologie di compressione, che è ridotto al minimo la quantità di dati inviati attraverso la rete. Questo supporto non è attualmente usato da qualsiasi prodotto Microsoft. |
| Estensione del commutatore piattaforma filtro Windows filtro leggeri|L'estensione del commutatore piattaforma filtro Windows filtro leggero consente agli sviluppatori di compilare [pacchetto di rete semplice le estensioni del commutatore virtuale Hyper-V di filtro](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering). È possibile ottenere le stesse funzionalità tramite la creazione di un'estensione di filtro completa. Di conseguenza, verrà rimosso questa estensione in futuro. |