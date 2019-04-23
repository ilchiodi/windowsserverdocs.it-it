---
title: Pianificare la sicurezza di Hyper-V in Windows Server
description: Fornisce un elenco di considerazioni sulla sicurezza per gli host Hyper-v e macchine virtuali
ms.prod: windows-server-threshold
ms.service: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 115db481-b57e-41c3-8354-504f4bc6113a
manager: dongill
author: larsiwer
ms.author: kathyDav
ms.date: 08/03/2018
ms.openlocfilehash: 462124e1907ef03aa7746dd050be3e8c84c7abda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877412"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>Pianificare la sicurezza di Hyper-V in Windows Server

>Si applica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Proteggere il sistema operativo host Hyper-V, le macchine virtuali, i file di configurazione e dati della macchina virtuale. Usare le seguenti procedure consigliate per un elenco di controllo per consentire di proteggere l'ambiente Hyper-V.

## <a name="secure-the-hyper-v-host"></a>Proteggere l'host Hyper-V
- **Mantenga l'host OS secure.**
    - Ridurre al minimo la superficie di attacco usando l'opzione di installazione minima di Windows Server necessari per il sistema operativo di gestione. Per altre informazioni, vedere la [sezione Opzioni di installazione](/windows-server/windows-server#installation-options) della raccolta di contenuto tecnica di Windows Server. Non è consigliabile eseguire i carichi di lavoro di produzione in Hyper-V in Windows 10.
    - Mantenere il sistema operativo host Hyper-V, firmware e driver di dispositivo aggiornati con gli ultimi aggiornamenti di sicurezza. Controllare le raccomandazioni del fornitore per l'aggiornamento del firmware e driver.
    - Non usare l'host Hyper-V come workstation o installare il software non necessari.
    - Gestire in remoto all'host Hyper-V. Se è necessario gestire l'host Hyper-V in locale, usare Credential Guard. Per altre informazioni, vedi [Proteggere le credenziali di dominio derivate con Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).
    - Abilitare i criteri di integrità di codice. Sicurezza basata su virtualizzazione Usa protetto da servizi di integrità del codice. Per altre informazioni, vedere [Guida alla distribuzione di Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
- **Usare una rete protetta.**
    - Usare una rete separata con una scheda di rete dedicate per il computer Hyper-V fisico.
    - Usare una rete privata o protetta per le configurazioni di macchina virtuale l'accesso e i file di disco rigido virtuale.
    - Usare una rete privata/dedicato per il traffico di migrazione in tempo reale. Provare ad abilitare IPSec su questa rete per usare la crittografia e proteggere i dati della VM trasmessi in rete durante la migrazione. Per altre informazioni, vedere [configurare gli host per live migration senza Clustering di Failover](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).
- **Proteggere il traffico di migrazione di archiviazione.** 

    Usare SMB 3.0 per la crittografia end-to-end dei dati SMB e la protezione dei dati, manomissione o intercettazione su reti non attendibili. Usare una rete privata per accedere al contenuto di condivisione SMB per impedire attacchi man-in-the-middle. Per altre informazioni, vedere [miglioramenti della sicurezza SMB](https://technet.microsoft.com/library/dn551363.aspx). 
- **Configurare gli host che devono fare parte di un'infrastruttura sorvegliata.** 

    Per altre informazioni, vedere [sorvegliato fabric](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
- **Proteggere i dispositivi.** 

    Proteggere i dispositivi di archiviazione in cui mantenere i file di risorse macchina virtuale.
    
- **Proteggere l'unità disco rigido.** 

    Usare crittografia unità BitLocker per proteggere le risorse.
    
- **Rafforzare il sistema operativo host Hyper-V.** 

    Usare le indicazioni di impostazione di sicurezza di base descritto nel [baseline della sicurezza di Windows Server](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Concedere le autorizzazioni appropriate.**
    - Aggiungere gli utenti che devono gestire l'host Hyper-V al gruppo amministratori Hyper-V.
    - Non concedere agli amministratori di macchine virtuali di autorizzazioni per il ruolo Hyper-V host del sistema operativo.

- **Configurare le esclusioni dell'antivirus e le opzioni per Hyper-V.**  

    Dispone già di Windows Defender [esclusioni automatiche](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus) configurato. Per altre informazioni sulle esclusioni, vedere [consiglia le esclusioni antivirus per gli host Hyper-V](https://support.microsoft.com/kb/3105657). 

- **Non montare dischi rigidi virtuali sconosciuti.** Questo può esporre l'attacchi a livello di file system dell'host.

- **Non abilitare la nidificazione nell'ambiente di produzione a meno che non è obbligatorio.**

    Se si abilita l'annidamento, non vengono eseguiti gli hypervisor non supportati in una macchina virtuale.  

Per gli ambienti più sicuri:

- **Usare hardware con un chip Trusted Platform Module (TPM) 2.0 per configurare un'infrastruttura sorvegliata.** 

    Per altre informazioni, vedere [requisiti di sistema per Hyper-V in Windows Server 2016](../system-requirements-for-hyper-v-on-windows.md).

## <a name="secure-virtual-machines"></a>Proteggere le macchine virtuali
- **Creazione di generazione 2 macchine virtuali per i sistemi operativi guest supportati.** 

    Per altre informazioni, vedere [le impostazioni di sicurezza di generazione 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Abilitare l'avvio protetto.** 

    Per altre informazioni, vedere [le impostazioni di sicurezza di generazione 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Mantenere il guest OS secure.**

    - Installare gli ultimi aggiornamenti di sicurezza prima di attivare una macchina virtuale in un ambiente di produzione.
    - Installare integration services per i sistemi operativi guest supportati che serve e mantenerlo aggiornato. Gli aggiornamenti di servizio di integrazione per gli utenti guest che eseguono versioni supportate di Windows sono disponibili tramite Windows Update.
    - Rafforzare il sistema operativo che viene eseguito in ogni macchina virtuale in base al ruolo che viene eseguita. Usare le raccomandazioni di impostazione di sicurezza della linea di base descritte nel [baseline della sicurezza di Windows](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Usare una rete protetta.** 

    Assicurarsi che le schede di rete virtuale di connettersi al commutatore virtuale appropriato e sono l'impostazione di sicurezza appropriati e limiti applicati.
    
- **Store dischi rigidi virtuali e i file di snapshot in un luogo sicuro.**

- **Proteggere i dispositivi.** 

    Configurare solo i dispositivi richiesti per una macchina virtuale. Non abilita l'assegnazione dispositivo discreti nell'ambiente di produzione a meno che non è necessario per uno scenario specifico. Se si abilita lo, assicurarsi di esporre solo i dispositivi da fornitori attendibili. 
    
- **Configurare antivirus, firewall e software di rilevamento delle intrusioni** all'interno delle macchine virtuali come appropriato base al ruolo di macchina virtuale.

- **Abilitare la sicurezza della virtualizzazione basata per gli utenti guest che eseguono Windows 10 o Windows Server 2016 o versione successiva.** 

    Per altre informazioni, vedere la [Guida alla distribuzione di Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
    
- **Abilitare solo discreti dispositivo assegnazione se è necessario per un carico di lavoro specifico**. 

    A causa della natura far passare attraverso un dispositivo fisico, rivolgersi al produttore del dispositivo per comprendere se deve essere usato in un ambiente sicuro.

Per gli ambienti più sicuri:

- **Distribuire macchine virtuali con abilitata la schermatura e distribuirli in un'infrastruttura sorvegliata.** 

    Per altre informazioni, vedere [le impostazioni di sicurezza di generazione 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md) e [sorvegliato fabric](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
