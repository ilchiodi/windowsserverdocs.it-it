---
title: Creare un gruppo di sicurezza per gli host sorvegliati e registrare il gruppo con HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: fb84720b94746a3c5757037ceb5c9bc8c965ff7f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447158"
---
# <a name="create-a-security-group-for-guarded-hosts-and-register-the-group-with-hgs"></a>Creare un gruppo di sicurezza per gli host sorvegliati e registrare il gruppo con HGS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

>[!IMPORTANT]
>Modalità di Active Directory è deprecata a partire da Windows Server 2019. Per gli ambienti in cui l'attestazione TPM non è possibile, configurare [ospitare l'attestazione chiave](guarded-fabric-initialize-hgs-key-mode.md). Attestazione chiave host fornisce una garanzia analoga alla modalità di Active Directory ed è più semplice da configurare. 


In questo argomento vengono descritti i passaggi intermedi per preparare gli host Hyper-V per acquisire gli host sorvegliati usando l'attestazione amministratore (modalità AD). Prima di eseguire questi passaggi, completare i passaggi descritti in [configurazione dell'infrastruttura DNS per gli host che diventeranno host sorvegliati](guarded-fabric-configuring-fabric-dns-ad.md).


## <a name="create-a-security-group-and-add-hosts"></a>Creare un gruppo di sicurezza e aggiungere gli host

1. Creare una nuova **GLOBAL** sicurezza gruppo nel dominio dell'infrastruttura e aggiungere gli host Hyper-V che eseguiranno le macchine virtuali schermate. Riavviare gli host per aggiornare l'appartenenza ai gruppi.

2. Utilizzare Get-ADGroup per ottenere l'ID di sicurezza (SID) del gruppo di sicurezza e fornire all'amministratore di HGS. 

    ```powershell
    Get-ADGroup "Guarded Hosts"
    ```

    ![Comando Get-AdGroup con output](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Registrare il SID del gruppo di sicurezza con HGS  

1. In un server HGS, eseguire il comando seguente per registrare il gruppo di sicurezza con HGS. 
   Eseguire nuovamente il comando se necessario per altri gruppi. 
   Specificare un nome descrittivo per il gruppo. 
   Non dovrà corrispondere al nome di gruppo di sicurezza Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Per verificare se è stato aggiunto al gruppo, eseguire [Get-HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Confermare l'attestazione](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Vedere anche

- [Il servizio sorveglianza Host per la distribuzione di host sorvegliati e macchine virtuali schermate](guarded-fabric-deploying-hgs-overview.md)
