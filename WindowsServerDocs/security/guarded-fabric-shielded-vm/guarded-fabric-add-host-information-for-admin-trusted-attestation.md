---
title: Aggiungere informazioni sull'host per l'attestazione attendibile dell'amministratore
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 946f91d05063475ae45fb334c67f8d5081d3984d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403718"
---
>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>Autorizzare gli host Hyper-V usando l'attestazione attendibile dell'amministratore

>[!IMPORTANT]
>L'attestazione amministratore (modalità AD) è deprecata a partire da Windows Server 2019. Per gli ambienti in cui non è possibile attestazione TPM, configurare l' [attestazione chiave host](guarded-fabric-initialize-hgs-key-mode.md). L'attestazione chiave host offre una garanzia simile alla modalità AD ed è più semplice da configurare. 


Per autorizzare un host sorvegliato in modalità AD: 

1. Nel dominio infrastruttura aggiungere gli host Hyper-V a un gruppo di sicurezza.
2. Nel dominio HGS registrare il SID del gruppo di sicurezza con HGS. 

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>Aggiungere l'host Hyper-V a un gruppo di sicurezza e riavviare l'host

1. Creare un gruppo di sicurezza **globale** nel dominio dell'infrastruttura e aggiungere gli host Hyper-V che eseguiranno le VM schermate. 
   Riavviare gli host per aggiornare l'appartenenza al gruppo.

2. Usare Get-ADGroup per ottenere l'ID di sicurezza (SID) del gruppo di sicurezza e fornirlo all'amministratore di HGS. 

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![Comando Get-AdGroup con output](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Registrare il SID del gruppo di sicurezza con HGS  

1. Ottenere il SID del gruppo di sicurezza per gli host sorvegliati dall'amministratore dell'infrastruttura ed eseguire il comando seguente per registrare il gruppo di sicurezza con HGS. 
   Eseguire di nuovo il comando, se necessario, per gruppi aggiuntivi. 
   Consente di specificare un nome descrittivo per il gruppo. 
   Non è necessario che corrisponda al nome del gruppo di sicurezza Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Per verificare che il gruppo sia stato aggiunto, eseguire [Get-HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 


