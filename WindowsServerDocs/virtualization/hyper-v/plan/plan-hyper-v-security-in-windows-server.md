---
title: Pianificare la sicurezza di Hyper-V in Windows Server
description: Fornisce un elenco di considerazioni sulla sicurezza per gli host Hyper-v e le macchine virtuali
ms.prod: windows-server
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
ms.openlocfilehash: 8fd86ae500fff1e6b8c27b0d34d1dcbeeade9f81
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392954"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>Pianificare la sicurezza di Hyper-V in Windows Server

>Si applica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019 Microsoft Hyper-V Server 2019

Proteggere il sistema operativo host Hyper-V, le macchine virtuali, i file di configurazione e i dati delle macchine virtuali. Usare l'elenco seguente di procedure consigliate come elenco di controllo per proteggere l'ambiente Hyper-V.

## <a name="secure-the-hyper-v-host"></a>Proteggere l'host Hyper-V
- **Proteggere il sistema operativo host.**
    - Ridurre la superficie di attacco utilizzando l'opzione di installazione minima di Windows Server necessaria per il sistema operativo di gestione. Per ulteriori informazioni, vedere la [sezione Opzioni di installazione](/windows-server/windows-server#installation-options) della raccolta di contenuti tecnici di Windows Server. Non è consigliabile eseguire carichi di lavoro di produzione in Hyper-V in Windows 10.
    - Per aggiornare il sistema operativo host Hyper-V, il firmware e i driver di dispositivo con gli aggiornamenti della sicurezza più recenti. Controllare le raccomandazioni del fornitore per aggiornare il firmware e i driver.
    - Non usare l'host Hyper-V come workstation o installare un software non necessario.
    - Gestire in remoto l'host Hyper-V. Se è necessario gestire l'host Hyper-V in locale, usare Credential Guard. Per altre informazioni, vedi [Proteggere le credenziali di dominio derivate con Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).
    - Abilitare i criteri di integrità del codice. Usare i servizi di integrità del codice protetti con sicurezza basata sulla virtualizzazione. Per altre informazioni, vedere la [Guida alla distribuzione di Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
- **Usare una rete sicura.**
    - Usare una rete separata con una scheda di rete dedicata per il computer Hyper-V fisico.
    - Usare una rete privata o protetta per accedere alle configurazioni delle macchine virtuali e ai file del disco rigido virtuale.
    - Usare una rete privata/dedicata per il traffico di migrazione in tempo reale. Si consiglia di abilitare IPSec in questa rete per usare la crittografia e proteggere i dati della macchina virtuale in rete durante la migrazione. Per altre informazioni, vedere [configurare gli host per la migrazione in tempo reale senza clustering di failover](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).
- **Traffico di migrazione archiviazione protetta.** 

    Usare SMB 3,0 per la crittografia end-to-end dei dati SMB e la manomissione o l'intercettazione della protezione dei dati su reti non attendibili. Usare una rete privata per accedere al contenuto della condivisione SMB per impedire attacchi man-in-the-Middle. Per ulteriori informazioni, vedere la pagina relativa ai miglioramenti apportati alla [sicurezza SMB](https://technet.microsoft.com/library/dn551363.aspx). 
- **Configurare gli host affinché facciano parte di un'infrastruttura sorvegliata.** 

    Per altre informazioni, vedere [infrastruttura sorvegliata](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
- **Proteggere i dispositivi.** 

    Proteggere i dispositivi di archiviazione in cui si conservano i file di risorse della macchina virtuale.
    
- **Proteggere il disco rigido.** 

    Usare Crittografia unità BitLocker per proteggere le risorse.
    
- **Rafforzare il sistema operativo host Hyper-V.** 

    Usare le indicazioni sulle impostazioni di sicurezza di base descritte nella [baseline della sicurezza di Windows Server](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Concedere le autorizzazioni appropriate.**
    - Aggiungere gli utenti che devono gestire l'host Hyper-V al gruppo Amministratori Hyper-V.
    - Non concedere le autorizzazioni di amministratore della macchina virtuale nel sistema operativo host Hyper-V.

- **Configurare le esclusioni e le opzioni antivirus per Hyper-V.**  

    Windows Defender dispone già di [esclusioni automatiche](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus) configurate. Per altre informazioni sulle esclusioni, vedere [esclusioni antivirus consigliate per gli host Hyper-V](https://support.microsoft.com/kb/3105657). 

- **Non montare dischi rigidi virtuali sconosciuti.** In questo modo è possibile esporre l'host a file system attacchi di livello.

- **Non abilitare l'annidamento nell'ambiente di produzione, a meno che non sia necessario.**

    Se si Abilita l'annidamento, non eseguire hypervisor non supportati in una macchina virtuale.  

Per ambienti più protetti:

- **Usare l'hardware con un chip Trusted Platform Module (TPM) 2,0 per configurare un'infrastruttura sorvegliata.** 

    Per ulteriori informazioni, vedere [requisiti di sistema per Hyper-V in Windows Server 2016](../system-requirements-for-hyper-v-on-windows.md).

## <a name="secure-virtual-machines"></a>Proteggere le macchine virtuali
- **Creare macchine virtuali di seconda generazione per i sistemi operativi guest supportati.** 

    Per altre informazioni, vedere [impostazioni di sicurezza di generazione 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Abilitare l'avvio protetto.** 

    Per altre informazioni, vedere [impostazioni di sicurezza di generazione 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md).
    
- **Proteggere il sistema operativo guest.**

    - Installare gli aggiornamenti della sicurezza più recenti prima di accendere una macchina virtuale in un ambiente di produzione.
    - Installare Integration Services per i sistemi operativi guest supportati che lo richiedono e mantenerli aggiornati. Gli aggiornamenti dei servizi di integrazione per i guest che eseguono versioni di Windows supportate sono disponibili tramite Windows Update.
    - Rafforzare il sistema operativo in esecuzione in ogni macchina virtuale in base al ruolo che esegue. Usare le indicazioni sulle impostazioni di sicurezza di base descritte nella [baseline della sicurezza di Windows](https://docs.microsoft.com/windows/device-security/windows-security-baselines).
    
- **Usare una rete sicura.** 

    Verificare che le schede di rete virtuali si connettano al commutire virtuale corretto e che siano applicate le impostazioni di sicurezza e i limiti appropriati.
    
- **Archiviare i dischi rigidi virtuali e i file di snapshot in un luogo sicuro.**

- **Proteggere i dispositivi.** 

    Configurare solo i dispositivi necessari per una macchina virtuale. Non abilitare l'assegnazione di dispositivi discreti nell'ambiente di produzione a meno che non sia necessaria per uno scenario specifico. Se si abilita questa operazione, assicurarsi di esporre solo i dispositivi da fornitori attendibili. 
    
- **Configurare il software antivirus, firewall e di rilevamento delle intrusioni** nelle macchine virtuali in base al ruolo della macchina virtuale.

- **Abilitare la sicurezza basata sulla virtualizzazione per i guest che eseguono Windows 10 o Windows Server 2016 o versioni successive.** 

    Per ulteriori informazioni, vedere la [Guida alla distribuzione di Device Guard](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide).
    
- **Abilitare l'assegnazione di dispositivi discreti solo se necessario per un carico di lavoro specifico**. 

    A causa della natura del passaggio attraverso un dispositivo fisico, collaborare con il produttore del dispositivo per capire se deve essere usato in un ambiente protetto.

Per ambienti più protetti:

- **Distribuire le macchine virtuali con la schermatura abilitata e distribuirle in un'infrastruttura sorvegliata.** 

    Per altre informazioni, vedere [impostazioni di sicurezza di generazione 2](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md) e [infrastruttura sorvegliata](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md).
