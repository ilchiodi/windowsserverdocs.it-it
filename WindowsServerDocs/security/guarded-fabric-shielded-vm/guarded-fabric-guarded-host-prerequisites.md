---
title: Prerequisiti per l'host sorvegliato
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5f2c3ec4b2c434ea945d86c4b1593e2e416a5123
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819232"
---
# <a name="prerequisites-for-guarded-hosts"></a>Prerequisiti per gli host sorvegliati

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Esaminare i prerequisiti di host per la modalità di attestazione scelta, quindi scegliere il passaggio successivo per aggiungere gli host sorvegliati.

## <a name="tpm-trusted-attestation"></a>Attestazione TPM

Gli host sorvegliati usando la modalità TPM devono soddisfare i prerequisiti seguenti:

-   **Hardware**: Un host è obbligatorio per la distribuzione iniziale. Per testare la migrazione in tempo reale di Hyper-V per le macchine virtuali schermate, è necessario disporre di almeno due host.

    Gli host devono avere:
    
    - IOMMU e Second Level Address Translation (SLAT)
    - TPM 2.0
    - Con hardware UEFI 2.3.1 o versione successiva
    - Configurato per l'avvio UEFI (non BIOS o modalità "legacy")
    - Avvio protetto abilitato
        
-   **Sistema operativo**: Windows Server 2016 Datacenter edition o versioni successive

    > [!IMPORTANT]
    > Assicurarsi di installare il [aggiornamento cumulativo più recente](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).  

-   **Ruolo e funzionalità**: Ruolo Hyper-V e la funzionalità di supporto Hyper-V per sorveglianza Host. Il supporto Hyper-V per sorveglianza Host funzionalità è disponibile solo nelle edizioni Datacenter di Windows Server. 

> [!WARNING]
> La funzionalità di supporto Hyper-V per sorveglianza Host abilita la protezione basata sulla virtualizzazione di integrità del codice che potrebbe non essere compatibile con alcuni dispositivi. È fortemente consigliabile testare questa configurazione nell'ambiente lab prima di abilitare questa funzionalità. In caso contrario, potrebbero verificarsi errori imprevisti, fino alla perdita di dati o all'errore con schermata blu (noto anche come errore irreversibile). Per altre informazioni, vedere [hardware compatibile con protezione basata su Windows Server Virtualization di integrità del codice](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Passaggio successivo:** 
>[!div class="nextstepaction"]
[Acquisire informazioni sul TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>Attestazione chiave host

Gli host sorvegliati usando l'attestazione chiave host devono soddisfare i prerequisiti seguenti:

- **Hardware**: Qualsiasi server in grado di eseguire l'inizio di Hyper-V con Windows Server 2019
- **Sistema operativo**: Windows Server 2019 Datacenter edition
- **Ruolo e funzionalità**: Ruolo Hyper-V e la funzionalità di supporto Hyper-V per sorveglianza Host 

L'host può essere unita a un dominio o un gruppo di lavoro. 

Per l'attestazione chiave host, HGS deve essere in esecuzione Windows Server 2019 e funziona con l'attestazione v2. Per altre informazioni, vedere [prerequisiti HGS](guarded-fabric-prepare-for-hgs.md#prerequisites). 

**Passaggio successivo:** 
>[!div class="nextstepaction"]
[Creare una coppia di chiavi](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>Attestazione amministratore

>[!IMPORTANT]
>Attestazione amministratore (modalità AD) è deprecato a partire da Windows Server 2019. Per gli ambienti in cui l'attestazione TPM non è possibile, configurare [ospitare l'attestazione chiave](#host-key-attestation). Attestazione chiave host fornisce una garanzia analoga alla modalità di Active Directory ed è più semplice da configurare. 

Host Hyper-V devono soddisfare i prerequisiti seguenti per la modalità di AD:

-   **Hardware**: Qualsiasi server in grado di eseguire l'inizio di Hyper-V con Windows Server 2016. Un host è obbligatorio per la distribuzione iniziale. Per testare la migrazione in tempo reale di Hyper-V per le macchine virtuali schermate, sono necessari almeno due host.

-   **Sistema operativo**: Windows Server 2016 Datacenter edition

    > [!IMPORTANT]
    > Installare il [aggiornamento cumulativo più recente](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).

-   **Ruolo e funzionalità**: Ruolo Hyper-V e la funzionalità supporto Hyper-V per sorveglianza Host, che è disponibile solo in Windows Server 2016 Datacenter edition. 

> [!WARNING]
> La funzionalità di supporto Hyper-V per sorveglianza Host abilita la protezione basata sulla virtualizzazione di integrità del codice che potrebbe non essere compatibile con alcuni dispositivi. È fortemente consigliabile testare questa configurazione nell'ambiente lab prima di abilitare questa funzionalità. In caso contrario, potrebbero verificarsi errori imprevisti, fino alla perdita di dati o all'errore con schermata blu (noto anche come errore irreversibile). Per altre informazioni, vedere [componenti hardware compatibili con protezione basata sulla virtualizzazione di Windows Server 2016 di integrità del codice](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Passaggio successivo:** 
>[!div class="nextstepaction"]
[Posizionare gli host sorvegliati in un gruppo di sicurezza](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)