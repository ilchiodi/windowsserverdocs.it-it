---
title: Windows Server, versione 1803 - Funzionalità rimosse
description: Informazioni sulle funzionalità che saranno rimosse o deprecate in Windows Server, versione 1803 o successiva
ms.prod: windows-server-threshold
ms.mktglfcycl: plan
ms.localizationpriority: medium
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.date: 05/10/2018
ms.openlocfilehash: c80738fe7ceda43a1a73adb0a8b1061bbb24319f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886202"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1803"></a>Funzionalità rimosse o pianificate per la sostituzione a partire da Windows Server, versione 1803

> Si applica a: Windows Server, versione 1803

In ogni versione di Windows Server vengono aggiunte nuove caratteristiche e funzionalità. Di tanto in tanto, vengono anche rimosse caratteristiche e funzionalità precedenti, sostituite con un'opzione migliore. Ecco i dettagli sulle caratteristiche e le funzionalità che sono state rimosse in Windows Server, versione 1803.   

> [!TIP]
> - Puoi accedere in anteprima alle build di Windows Server partecipando al [programma Windows Insider](https://insider.windows.com): è un ottimo modo per sperimentare le modifiche relative alle funzionalità.
> - Se hai domande su altre versioni, vedi gli articoli relativi a [Windows Server 2016](deprecated-features.md)e [Windows Server, versione 1709](removed-features-1709.md).

**L'elenco è soggetto a modifiche e potrebbe non includere tutte le funzionalità interessate o funzionalità.** 

## <a name="features-we-removed-in-this-release"></a>Funzionalità rimosse in questa versione

Abbiamo rimosso le seguenti caratteristiche e funzionalità dall'immagine del prodotto installato in Windows Server, versione 1803. Il codice o le applicazioni che dipendono da tali funzionalità non funzioneranno in questa versione, a meno che non venga utilizzato un metodo alternativo.   

|Funzionalità    |In alternativa puoi utilizzare...|
|-----------|--------------------|
|[Servizio Replica file](https://support.microsoft.com/en-us/help/4025991/windows-server-version-1709-no-longer-supports-frs)|Il servizio Replica file, introdotto in Windows Server 2003 R2, è stato sostituito da Replica DFS. Devi [eseguire la migrazione dei controller di dominio che utilizzano il servizio Replica file a Replica DFS con SYSVOL](https://blogs.technet.microsoft.com/filecab/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol/).|
|Virtualizzazione rete Hyper-V (HNV)|La [virtualizzazione di rete](../networking/sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md) è ora inclusa in Windows Server come parte della soluzione [Software Defined Networking](../networking/sdn/software-defined-networking.md) (SDN), che include anche il controller di rete, il bilanciamento del carico software, il routing definito dall'utente e gli elenchi di controllo di accesso.|

## <a name="features-were-no-longer-developing"></a>Funzionalità di cui è cessato lo sviluppo

Le funzionalità elencate di seguito non vengono più sviluppate e potranno essere rimosse con un aggiornamento futuro. Alcune sono state sostituite con altre caratteristiche o funzionalità, mentre altre sono ora disponibili da origini differenti. 

>[!NOTE]
> Tieni presente che alcuni dei ruoli e delle funzionalità descritti di seguito non sono inclusi nell'opzione di installazione dei componenti di base, fornita in Windows Server, versione 1803. *Sono* invece presenti nell'opzione di installazione Server con Esperienza Desktop, il cui ultimo rilascio risale a Windows Server 2016 e che verrà nuovamente rilasciata con Windows Server 2019.

Se hai commenti e suggerimenti da condividere sulla sostituzione proposta di una di queste funzionalità, puoi utilizzare l'[app Hub di Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app). 

|Ruolo o funzionalità    |In alternativa puoi utilizzare...|
|-----------|---------------------|
|Digitalizzazione aziendale, anche denominata Gestione digitalizzazione distribuita|La [funzionalità Gestione digitalizzazione](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759124\(v%3dws.11\)) è stata introdotta in Windows Server 2008 R2 e consentiva la digitalizzazione e la gestione degli scanner nell'azienda in condizioni di sicurezza. Non stiamo più investendo in questa funzionalità e non sono presenti dispositivi che la supportano.|
|Tecnologie di transizione IPv4/6 (6to4, ISATAP e tunnel diretti)|La funzionalità 6to4 è stata disabilitata per impostazione predefinita a partire da Windows 10, versione 1607 (aggiornamento dell'anniversario), la funzionalità ISATAP è stata disabilitata per impostazione predefinita a partire da Windows 10, versione 1703 (Creators Update), e la funzionalità dei tunnel diretti è sempre stata disabilitata per impostazione predefinita. In alternativa utilizza il supporto di IPv6 nativo.|
|[MultiPoint Services](../remote/multipoint-services/multipoint-services.md)|Abbiamo cessato lo sviluppo del ruolo MultiPoint Services come parte di Windows Server. I servizi MultiPoint Connector sono disponibili tramite [Funzionalità su richiesta](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities) sia per Windows Server che per Windows 10. È possibile utilizzare [Servizi Desktop remoto](../remote/remote-desktop-services/welcome-to-rds.md), in particolare Host sessione Desktop remoto, per fornire la connettività RDP. |
|[Pacchetti di simboli offline](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugger-download-symbols) (file MSI dei simboli di debug)|I pacchetti di simboli non vengono più resi disponibili come file MSI scaricabili. È in corso la transizione dal [server dei simboli Microsoft a un archivio dei simboli basato su Azure](https://blogs.msdn.microsoft.com/windbg/2017/10/18/update-on-microsofts-symbol-server/). Se devi utilizzare i simboli di Windows, connettiti al server dei simboli Microsoft per memorizzare i simboli nella cache locale o utilizza un file manifesto con SymChk.exe in un computer con accesso a Internet.|
|[Gestore connessione Desktop remoto e Host di virtualizzazione Desktop remoto](../remote/remote-desktop-services/desktop-hosting-service.md) in un'installazione dei componenti di base del server|Nella maggior parte delle distribuzioni di Servizi Desktop remoto, il percorso di questi ruoli è condiviso con Host sessione Desktop remoto, che richiede Server con Esperienza desktop; pertanto, per garantire la coerenza con Host sessione Desktop remoto, stiamo apportando modifiche ai ruoli in questione imponendo anche per essi il requisito di Server con Esperienza desktop. Abbiamo cessato lo sviluppo di questi ruoli di Servizi Desktop remoto per l'uso in un'[installazione dei componenti di base del server](../administration/server-core/what-is-server-core.md). Se desideri [distribuire questi ruoli nell'ambito dell'infrastruttura di Desktop remoto](../remote/remote-desktop-services/rds-deploy-infrastructure.md), puoi [installarli in Windows Server 2016 con Esperienza desktop](getting-started-with-server-with-desktop-experience.md). <br/><br/>Questi ruoli sono inclusi anche nell'opzione di installazione Esperienza desktop di Windows Server 2019. Puoi testarli nella [build di Windows Server 2019 disponibile con Windows Insider](https://docs.microsoft.com/windows-insider/at-work/), assicurandoti di selezionare l'immagine LTSC. |
|[RemoteFX vGPU](../remote/remote-desktop-services/rds-remotefx-vgpu.md)|Stiamo sviluppando nuove opzioni di accelerazione grafica per gli ambienti virtualizzati. In alternativa, puoi anche utilizzare l'[assegnazione di dispositivi discreti](../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md).|
|[Criteri di restrizione software](../identity/software-restriction-policies/software-restriction-policies.md) in Criteri di gruppo|Anziché utilizzare Criteri di restrizione software tramite Criteri di gruppo, puoi utilizzare [AppLocker](https://docs.microsoft.com/windows/security/threat-protection/applocker/applocker-overview) o [Controllo di applicazioni di Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control) per stabilire a quali applicazioni possono accedere gli utenti e quale codice può essere eseguito nel kernel.|
|Spazi di archiviazione in una configurazione condivisa con un'infrastruttura SAS|In alternativa, distribuisci [Spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md). Spazi di archiviazione diretta supporta l'utilizzo di alloggiamenti SAS con certificazione HLK, ma in una configurazione non condivisa, come descritto in [Requisiti hardware di Spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).|
|Esperienza Windows Server Essentials|Abbiamo cessato lo sviluppo del ruolo Essentials Experience per le SKU di Windows Server Standard o Windows Server Datacenter. Se desideri una soluzione server di facile utilizzo per le piccole e medie imprese, prova la nuova soluzione [Microsoft 365 per l'azienda](https://www.microsoft.com/microsoft-365/business) o utilizza [Windows Server 2016 Essentials](https://docs.microsoft.com/windows-server-essentials/get-started/get-started).|

