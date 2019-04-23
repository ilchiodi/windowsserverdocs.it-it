---
title: Distribuire host protetti
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3b20a7eb2b5097d8ddb7381fd0304581ca4e6722
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845352"
---
# <a name="deploy-guarded-hosts"></a>Distribuire host protetti

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Gli argomenti in questa sezione descrivono i passaggi che richiede un amministratore dell'infrastruttura per configurare gli host Hyper-V per lavorare con il servizio sorveglianza Host (HGS). Prima di iniziare questa procedura, almeno un nodo nel [HGS cluster deve essere impostato](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

**Per l'attestazione TPM**:
1. [Configurare l'infrastruttura DNS](guarded-fabric-configuring-fabric-dns.md): Viene descritto come configurare un server d'inoltro DNS dal dominio dell'infrastruttura per il dominio del servizio HGS.
2. [Acquisire le informazioni richieste dal servizio HGS](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md): Indica come acquisire gli identificatori TPM (detti anche gli identificatori della piattaforma), creare un criterio di integrità del codice e creare una linea di base TPM. Fornirà quindi queste informazioni all'amministratore di HGS per configurare l'attestazione.
3. [Verificare che consente di attestare gli host sorvegliati](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**Per l'attestazione chiave host**:
1. [Creare una chiave host](guarded-fabric-create-host-key.md#create-a-host-key): Viene descritto come configurare un server d'inoltro DNS dal dominio dell'infrastruttura per il dominio del servizio HGS.
2. [Aggiungere il codice Product key host per il servizio di attestazione](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service): Viene descritto come configurare un gruppo di sicurezza di Active Directory nel dominio dell'infrastruttura, aggiungere gli host sorvegliati come membri di tale gruppo e fornire tale identificatore di gruppo all'amministratore del servizio HGS. 
3. [Verificare che consente di attestare gli host sorvegliati](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**Per l'attestazione amministratore**:
1. [Configurare l'infrastruttura DNS](guarded-fabric-configuring-fabric-dns.md): Viene descritto come configurare un server d'inoltro DNS dal dominio dell'infrastruttura per il dominio del servizio HGS.
2. [Creare un gruppo di sicurezza](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md): Viene descritto come configurare un gruppo di sicurezza di Active Directory nel dominio dell'infrastruttura, aggiungere gli host sorvegliati come membri di tale gruppo e fornire tale identificatore di gruppo all'amministratore del servizio HGS. 
3. [Verificare che consente di attestare gli host sorvegliati](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Vedere anche

- [Attività di distribuzione per le infrastrutture protetta e macchine virtuali schermate](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
