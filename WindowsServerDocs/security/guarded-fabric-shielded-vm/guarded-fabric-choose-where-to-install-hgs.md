---
title: Scegliere se installare HGS nella propria nuova foresta o in una foresta bastion esistente
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 4e02cd37391e629c9b947095fe32626bd15726ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827502"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>Scegliere se installare HGS nella propria foresta dedicata o in una foresta bastion esistente

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016


La foresta di Active Directory per HGS è riservata perché gli amministratori hanno accesso alle chiavi che controllo di macchine virtuali schermate. L'installazione predefinita verrà impostata una nuova foresta dedicata per HGS e configurare altre dipendenze. Questa opzione è consigliata perché l'ambiente è indipendente e Nota per essere protetti quando viene creato. 

Il requisito tecnico solo per l'installazione di HGS in una foresta esistente è che deve essere aggiunto al dominio radice. domini non radice non sono supportati. Ma esistono anche i requisiti operativi e le procedure consigliate relative alla sicurezza per l'uso di una foresta esistente. Foreste adatte intenzionalmente create per servire una funzione sensibile, ad esempio l'insieme di strutture utilizzate da [Privileged Access Management per Active Directory Domain Services](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) o un [foresta migliorata sicurezza amministrativa ambiente (ESAE)](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access-reference-material#ESAE_BM). Queste foreste si comportano in genere le caratteristiche seguenti:

- Sono presenti alcuni amministratori (separati dagli amministratori dell'infrastruttura)
- Hanno un numero ridotto di accessi riusciti
- Non sono per utilizzo generico di natura 

Foreste generico, ad esempio gli insiemi di strutture di produzione non sono adatte per l'uso da HGS. Le foreste dell'infrastruttura sono non adatto anche perché HGS deve essere isolato dagli amministratori dell'infrastruttura.

## <a name="next-step"></a>Passaggio successivo

Scegliere l'opzione di installazione più adatto dell'ambiente:

- [Installare servizio HGS nella propria foresta dedicata](guarded-fabric-install-hgs-default.md)
- [Installare servizio HGS in una foresta bastion esistente](guarded-fabric-install-hgs-in-a-bastion-forest.md)


