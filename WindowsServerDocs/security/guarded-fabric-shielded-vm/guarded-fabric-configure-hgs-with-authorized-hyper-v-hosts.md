---
title: Distribuire host protetti
ms.prod: windows-server
ms.topic: article
ms.assetid: 2379ca26-b32d-4055-8b4b-99d1f2df37e1
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 0f678172d397ff61fd336b7c844d43f77bea7fad
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856834"
---
# <a name="deploy-guarded-hosts"></a>Distribuire host protetti

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Negli argomenti di questa sezione vengono descritti i passaggi necessari a un amministratore di infrastruttura per configurare gli host Hyper-V per l'utilizzo con il servizio sorveglianza host (HGS). Prima di poter avviare questi passaggi, è necessario configurare almeno un nodo nel [cluster HGS](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

**Per l'attestazione TPM**:
1. [Configurare il DNS dell'infrastruttura](guarded-fabric-configuring-fabric-dns.md): indica come configurare un server d'inoltre DNS dal dominio dell'infrastruttura al dominio HGS.
2. [Informazioni di acquisizione richieste da HGS](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md): indica come acquisire gli identificatori TPM (detti anche identificatori di piattaforma), creare un criterio di integrità del codice e creare una baseline TPM. Queste informazioni vengono quindi fornite all'amministratore di HGS per configurare l'attestazione.
3. [Confermare che gli host sorvegliati possono attestare](guarded-fabric-confirm-hosts-can-attest-successfully.md)

**Per l'attestazione chiave host**:
1. [Creare una chiave host](guarded-fabric-create-host-key.md#create-a-host-key): indica come configurare un server d'inoltre DNS dal dominio dell'infrastruttura al dominio HGS.
2. [Aggiungere la chiave host al servizio di attestazione](guarded-fabric-create-host-key.md#add-the-host-key-to-the-attestation-service): indica come configurare un gruppo di sicurezza Active Directory nel dominio dell'infrastruttura, aggiungere host sorvegliati come membri del gruppo e specificare l'identificatore di gruppo per l'amministratore di HGS. 
3. [Confermare che gli host sorvegliati possono attestare](guarded-fabric-confirm-hosts-can-attest-successfully.md)


**Per l'attestazione attendibile dell'amministratore**:
1. [Configurare il DNS dell'infrastruttura](guarded-fabric-configuring-fabric-dns.md): indica come configurare un server d'inoltre DNS dal dominio dell'infrastruttura al dominio HGS.
2. [Creare un gruppo di sicurezza](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md): indica come configurare un gruppo di sicurezza Active Directory nel dominio dell'infrastruttura, aggiungere host sorvegliati come membri del gruppo e specificare l'identificatore di gruppo per l'amministratore di HGS. 
3. [Confermare che gli host sorvegliati possono attestare](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Vedere anche

- [Attività di distribuzione per infrastrutture sorvegliate e VM schermate](guarded-fabric-deploying-hgs-overview.md#deployment-tasks-for-guarded-fabrics-and-shielded-vms)
