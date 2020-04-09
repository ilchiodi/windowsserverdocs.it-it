---
title: Scegliere se installare HGS nella relativa nuova foresta o in una foresta Bastion esistente
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 52376a4e193b8021dc58214003e9b7579b096a79
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856884"
---
# <a name="choose-whether-to-install-hgs-in-its-own-dedicated-forest-or-in-an-existing-bastion-forest"></a>Scegliere se installare HGS nella propria foresta dedicata o in una foresta Bastion esistente

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016


La foresta Active Directory per HGS è sensibile perché gli amministratori hanno accesso alle chiavi che controllano le macchine virtuali schermate. Con l'installazione predefinita viene configurata una nuova foresta dedicata per HGS e vengono configurate altre dipendenze. Questa opzione è consigliata perché l'ambiente è indipendente e noto come protetto quando viene creato. 

L'unico requisito tecnico per l'installazione di HGS in una foresta esistente è che viene aggiunto al dominio radice. i domini non radice non sono supportati. Tuttavia, esistono anche requisiti operativi e procedure consigliate relative alla sicurezza per l'utilizzo di una foresta esistente. Le foreste appropriate vengono create appositamente per servire una funzione sensibile, ad esempio la foresta usata da [Privileged Access Management per servizi di dominio Active Directory](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) o una [foresta ESAE (Enhanced Security Administrative Environment)](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access-reference-material#ESAE_BM). Tali foreste presentano in genere le caratteristiche seguenti:

- Hanno pochi amministratori (distinti dagli amministratori dell'infrastruttura)
- Hanno un numero ridotto di accessi
- Non sono di natura generale 

Le foreste di uso generico come le foreste di produzione non sono adatte per l'uso da HGS. Anche le foreste dell'infrastruttura non sono adatte perché HGS deve essere isolato dagli amministratori dell'infrastruttura.

## <a name="next-step"></a>Passaggio successivo

Scegliere l'opzione di installazione più adatta al proprio ambiente:

- [Installare HGS nella propria foresta dedicata](guarded-fabric-install-hgs-default.md)
- [Installare HGS in una foresta Bastion esistente](guarded-fabric-install-hgs-in-a-bastion-forest.md)


