---
title: Funzionalità rimosse o pianificate per la rimozione in Windows Server 2019
description: Informazioni sulle caratteristiche e le funzionalità rimosse o pianificate per la rimozione a partire da Windows Server 2019.
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
ms.openlocfilehash: a27fb6bfa0d96d7d3cdd0b9b5bc4912b12b4462b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280365"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>Funzionalità rimosse o pianificate per la sostituzione a partire da Windows Server 2019

>Si applica a: Windows Server 2019

In ogni versione di Windows Server vengono aggiunte nuove caratteristiche e funzionalità. Occasionalmente rimuoviamo alcune caratteristiche e funzionalità, in genere perché abbiamo aggiunto un'opzione migliore. Ecco i dettagli sulle caratteristiche e le funzionalità che abbiamo rimosso in Windows Server 2019.

> [!TIP]
> - Puoi accedere in anteprima alle build di Windows Server partecipando al [Programma Windows Insider](https://insider.windows.com), un ottimo modo per sperimentare le modifiche relative alle funzionalità.
> - Se hai domande su altre versioni, leggi le informazioni contenute in [Novità di Windows Server](../get-started/whats-new-in-windows-server.md).

**L'elenco è soggetto a modifiche e potrebbe non includere tutte le caratteristiche o le funzionalità interessate.** 

## <a name="features-we-removed-in-this-release"></a>Funzionalità rimosse in questa versione

È in corso la rimozione delle caratteristiche e delle funzionalità seguenti dall'immagine del prodotto installato in Windows Server 2019. Il codice o le applicazioni che dipendono da esse non funzioneranno in questa versione, a meno che non usi un metodo alternativo.

|Funzionalità    |In alternativa puoi usare...|
|-----------|--------------------
|Digitalizzazione aziendale, denominata anche Gestione digitalizzazione distribuita|È corso la rimozione di questa funzionalità di digitalizzazione e gestione dello scanner in condizioni di sicurezza. Non esistono dispositivi che supportano questa funzionalità.|
|Componenti di stampa, ora componenti facoltativi per le installazioni Server Core|Nelle versioni precedenti di Windows Server i componenti di stampa sono *disabilitati* per impostazione predefinita nell'opzione di installazione Server Core. In Windows Server 2016 questo è cambiato e tali componenti sono stati abilitati per impostazione predefinita. In Windows Server 2019 questi componenti di stampa sono di nuovo disabilitati per impostazione predefinita per Server Core. Per abilitare i componenti di stampa, puoi eseguire il cmdlet **Install-WindowsFeature Print-Server**.|
|[Gestore connessione Desktop remoto e Host di virtualizzazione Desktop remoto](../remote/remote-desktop-services/desktop-hosting-service.md) in un'installazione Server Core|Nella maggior parte delle distribuzioni di Servizi Desktop remoto il percorso di questi ruoli è condiviso con Host sessione Desktop remoto, che richiede il server con Esperienza desktop. Pertanto, per garantire la coerenza con Host sessione Desktop remoto, stiamo apportando modifiche a questi ruoli imponendo anche per essi il requisito del server con Esperienza desktop. Questi ruoli di Servizi Desktop remoto non sono più disponibili per l'uso in un'[installazione Server Core](../administration/server-core/what-is-server-core.md). Se vuoi [distribuire questi ruoli nell'ambito dell'infrastruttura di Desktop remoto](../remote/remote-desktop-services/rds-deploy-infrastructure.md), puoi [installarli in Windows Server con Esperienza desktop](../get-started/getting-started-with-server-with-desktop-experience.md). <br/><br/>Questi ruoli sono inclusi anche nell'opzione di installazione Esperienza desktop di Windows Server 2019. |

## <a name="features-were-no-longer-developing"></a>Funzionalità di cui è cessato lo sviluppo

Non stiamo più sviluppando le funzionalità elencate di seguito e potremmo rimuoverle da un aggiornamento futuro. Alcune sono state sostituite con altre caratteristiche o funzionalità, mentre altre sono ora disponibili da origini differenti. 

Se hai feedback da condividere sulla sostituzione proposta di una di queste funzionalità, puoi usare l'[app Hub di Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

| Funzionalità   | In alternativa puoi usare... |
|-----------|---------------------|
| Unità di archiviazione chiavi in Hyper-V|Non lavoriamo più alla funzionalità Unità di archiviazione chiavi in Hyper-V. Se usi macchine virtuali di prima generazione, consulta [Sicurezza della virtualizzazione delle macchine virtuali di prima generazione](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v) per informazioni sulle opzioni attive. Se hai intenzione di creare nuove macchine virtuali, usa macchine virtuali di seconda generazione con dispositivi TPM per una soluzione più sicura. |
| Console di gestione Trusted Platform Module (TPM)|Le informazioni disponibili in precedenza nella console di gestione TPM sono ora disponibili nella pagina [**Sicurezza dei dispositivi**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security) di [Windows Defender Security Center](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center). |
| Modalità di attestazione Active Directory del servizio Sorveglianza host|Non ci occupiamo più dello sviluppo della modalità di attestazione Active Directory del servizio Sorveglianza host. Abbiamo aggiunto invece una nuova modalità di attestazione, l'[attestazione chiavi host](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md), che è molto più semplice e altrettanto compatibile dell'attestazione basata su Active Directory.  Questa nuova modalità fornisce una funzionalità equivalente con un'esperienza di installazione, una gestione più semplice e meno dipendenze dell'infrastruttura rispetto all'attestazione Active Directory. L'attestazione chiavi host non prevede requisiti hardware aggiuntivi oltre a quelli previsti dall'attestazione Active Directory, pertanto tutti i sistemi esistenti continueranno a essere compatibili con la nuova modalità. Vedi [Distribuire host sorvegliati](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md) per altre informazioni sulle opzioni di attestazione. |
| Servizio OneSync|Il servizio OneSync sincronizza i dati per le app Posta, Calendari e Contatti. Abbiamo aggiunto un motore di sincronizzazione per l'app Outlook che fornisce la stessa sincronizzazione. |
| Supporto dell'API Remote Differential Compression|Il supporto dell'API Remote Differential Compression permette la sincronizzazione dei dati con un'origine remota mediante tecnologie di compressione, riducendo al minimo la quantità di dati inviati attraverso la rete. |
| Estensione commutatore filtro LWF Piattaforma filtro Windows|L'estensione commutatore filtro LWF Piattaforma filtro Windows permette agli sviluppatori di creare [estensioni di filtro di pacchetti di rete semplici per il commutatore virtuale Hyper-V](https://docs.microsoft.com/windows-hardware/drivers/network/using-virtual-switch-filtering). Poiché puoi ottenere la stessa funzionalità creando un'estensione di filtro completa, rimuoveremo questa estensione in futuro. |
