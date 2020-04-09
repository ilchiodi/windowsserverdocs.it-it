---
title: Prerequisiti host sorvegliati
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: fdd97b90ccfe770564e834f54461a31e2c41a2ea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856704"
---
# <a name="prerequisites-for-guarded-hosts"></a>Prerequisiti per gli host sorvegliati

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Esaminare i prerequisiti host per la modalità di attestazione scelta, quindi fare clic sul passaggio successivo per aggiungere gli host sorvegliati.

## <a name="tpm-trusted-attestation"></a>Attestazione TPM

Gli host sorvegliati che usano la modalità TPM devono soddisfare i seguenti prerequisiti:

-   **Hardware**: è necessario un host per la distribuzione iniziale. Per testare la migrazione in tempo reale di Hyper-V per le macchine virtuali schermate, è necessario disporre di almeno due host.

    Gli host devono avere:
    
    - IOMMU e Second Level Address Translation (stecca)
    - TPM 2.0
    - UEFI 2.3.1 o versione successiva
    - Configurato per l'avvio tramite UEFI (non BIOS o modalità "Legacy")
    - Avvio protetto abilitato
        
-   **Sistema operativo**: Windows Server 2016 Datacenter Edition o versione successiva

    > [!IMPORTANT]
    > Assicurarsi di installare l' [aggiornamento cumulativo più recente](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).  

-   **Ruolo e funzionalità**: ruolo Hyper-v e funzionalità di supporto Hyper-v per sorveglianza host. La funzionalità di supporto per sorveglianza host per Hyper-V è disponibile solo nelle edizioni Datacenter di Windows Server. 

> [!WARNING]
> La funzionalità di supporto per sorveglianza host di Hyper-V consente la protezione basata sulla virtualizzazione dell'integrità del codice che potrebbe non essere compatibile con alcuni dispositivi. È consigliabile testare questa configurazione nel Lab prima di abilitare questa funzionalità. In caso contrario, potrebbero verificarsi errori imprevisti, fino alla perdita di dati o all'errore con schermata blu (noto anche come errore irreversibile). Per ulteriori informazioni, vedere [hardware compatibile con la protezione basata sulla virtualizzazione di Windows Server per l'integrità del codice](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Passaggio successivo:** 
> [!div class="nextstepaction"]
> [Acquisisci informazioni TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>Attestazione chiave host

Gli host sorvegliati che usano l'attestazione chiave host devono soddisfare i seguenti prerequisiti:

- **Hardware**: qualsiasi server in grado di eseguire Hyper-V a partire da Windows Server 2019
- **Sistema operativo**: Windows Server 2019 Datacenter Edition
- **Ruolo e funzionalità**: ruolo Hyper-v e funzionalità di supporto Hyper-v per sorveglianza host 

L'host può essere aggiunto a un dominio o a un gruppo di lavoro. 

Per l'attestazione della chiave host, HGS deve eseguire Windows Server 2019 e funzionare con l'attestazione V2. Per ulteriori informazioni, vedere [prerequisiti di HGS](guarded-fabric-prepare-for-hgs.md#prerequisites). 

**Passaggio successivo:** 
> [!div class="nextstepaction"]
> [Creare una coppia di chiavi](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>Attestazione amministratore

>[!IMPORTANT]
>L'attestazione amministratore (modalità AD) è deprecata a partire da Windows Server 2019. Per gli ambienti in cui non è possibile attestazione TPM, configurare l' [attestazione chiave host](#host-key-attestation). L'attestazione chiave host offre una garanzia simile alla modalità AD ed è più semplice da configurare. 

Gli host Hyper-V devono soddisfare i seguenti prerequisiti per la modalità AD:

-   **Hardware**: qualsiasi server in grado di eseguire Hyper-V a partire da Windows Server 2016. Per la distribuzione iniziale è necessario un host. Per testare la migrazione in tempo reale di Hyper-V per le macchine virtuali schermate, sono necessari almeno due host.

-   **Sistema operativo**: Windows Server 2016 Datacenter Edition

    > [!IMPORTANT]
    > Installare l' [aggiornamento cumulativo più recente](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).

-   **Ruolo e funzionalità**: ruolo Hyper-v e la funzionalità di supporto Hyper-v per sorveglianza host, disponibile solo in Windows Server 2016 Datacenter Edition. 

> [!WARNING]
> La funzionalità di supporto per sorveglianza host di Hyper-V consente la protezione basata sulla virtualizzazione dell'integrità del codice che potrebbe non essere compatibile con alcuni dispositivi. È consigliabile testare questa configurazione nel Lab prima di abilitare questa funzionalità. In caso contrario, potrebbero verificarsi errori imprevisti, fino alla perdita di dati o all'errore con schermata blu (noto anche come errore irreversibile). Per ulteriori informazioni, vedere [hardware compatibile con la protezione basata sulla virtualizzazione di Windows Server 2016 per l'integrità del codice](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Passaggio successivo:** 
> [!div class="nextstepaction"]
> [Inserire host sorvegliati in un gruppo di sicurezza](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)