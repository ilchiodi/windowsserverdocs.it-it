---
title: Creare un gruppo di sicurezza per gli host sorvegliati e registrare il gruppo con HGS
ms.prod: windows-server
ms.topic: article
ms.assetid: a12c8494-388c-4523-8d70-df9400bbc2c0
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b29f8bb4bfb8b2b685a6c4ec1a1d2965d8fbde58
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856944"
---
# <a name="create-a-security-group-for-guarded-hosts-and-register-the-group-with-hgs"></a>Creare un gruppo di sicurezza per gli host sorvegliati e registrare il gruppo con HGS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

>[!IMPORTANT]
>La modalità Active Directory è deprecata a partire da Windows Server 2019. Per gli ambienti in cui non è possibile attestazione TPM, configurare l' [attestazione chiave host](guarded-fabric-initialize-hgs-key-mode.md). L'attestazione chiave host offre una garanzia simile alla modalità AD ed è più semplice da configurare. 


Questo argomento descrive i passaggi intermedi per preparare gli host Hyper-V per diventare host sorvegliati usando l'attestazione attendibile dell'amministratore (modalità AD). Prima di eseguire questi passaggi, seguire la procedura descritta in [configurazione del DNS di infrastruttura per gli host che diventeranno host sorvegliati](guarded-fabric-configuring-fabric-dns-ad.md).


## <a name="create-a-security-group-and-add-hosts"></a>Creare un gruppo di sicurezza e aggiungere host

1. Creare un nuovo gruppo di sicurezza **globale** nel dominio dell'infrastruttura e aggiungere gli host Hyper-V che eseguiranno le VM schermate. Riavviare gli host per aggiornare l'appartenenza al gruppo.

2. Usare Get-ADGroup per ottenere l'ID di sicurezza (SID) del gruppo di sicurezza e fornirlo all'amministratore di HGS. 

    ```powershell
    Get-ADGroup "Guarded Hosts"
    ```

    ![Comando Get-AdGroup con output](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Registrare il SID del gruppo di sicurezza con HGS  

1. In un server HGS eseguire il comando seguente per registrare il gruppo di sicurezza con HGS. 
   Eseguire di nuovo il comando, se necessario, per gruppi aggiuntivi. 
   Consente di specificare un nome descrittivo per il gruppo. 
   Non è necessario che corrisponda al nome del gruppo di sicurezza Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Per verificare che il gruppo sia stato aggiunto, eseguire [Get-HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Confermare l'attestazione](guarded-fabric-confirm-hosts-can-attest-successfully.md)


## <a name="see-also"></a>Vedere anche

- [Distribuzione del servizio sorveglianza host per host sorvegliati e macchine virtuali schermate](guarded-fabric-deploying-hgs-overview.md)
