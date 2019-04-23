---
title: Aggiungere informazioni sull'host per l'attestazione di attendibilità dell'amministratore
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 87089ebc-b953-4aa3-96b5-966cf91acb02
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 7949711dbb0f89f5404b491d60938985bfa98c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849462"
---
>Si applica a: Windows Server (canale semestrale), Windows Server 2016

# <a name="authorize-hyper-v-hosts-using-admin-trusted-attestation"></a>Autorizzare gli host Hyper-V usando l'attestazione dell'attendibilità dell'amministratore

>[!IMPORTANT]
>Attestazione amministratore (modalità AD) è deprecato a partire da Windows Server 2019. Per gli ambienti in cui l'attestazione TPM non è possibile, configurare [ospitare l'attestazione chiave](guarded-fabric-initialize-hgs-key-mode.md). Attestazione chiave host fornisce una garanzia analoga alla modalità di Active Directory ed è più semplice da configurare. 


Per autorizzare un host sorvegliato in modalità AD: 

1. Nel dominio dell'infrastruttura, aggiungere gli host Hyper-V a un gruppo di sicurezza.
2. Nel dominio HGS, registrare il SID del gruppo di sicurezza con HGS. 

## <a name="add-the-hyper-v-host-to-a-security-group-and-reboot-the-host"></a>Aggiungere l'host Hyper-V a un gruppo di sicurezza e riavviare l'host

1. Creare un **GLOBAL** sicurezza gruppo nel dominio dell'infrastruttura e aggiungere gli host Hyper-V che eseguiranno le macchine virtuali schermate. 
   Riavviare gli host per aggiornare l'appartenenza ai gruppi.

2. Utilizzare Get-ADGroup per ottenere l'ID di sicurezza (SID) del gruppo di sicurezza e fornire all'amministratore di HGS. 

   ```powershell
   Get-ADGroup "Guarded Hosts"
   ```

   ![Comando Get-AdGroup con output](../media/Guarded-Fabric-Shielded-VM/guarded-host-get-adgroup.png)

## <a name="register-the-sid-of-the-security-group-with-hgs"></a>Registrare il SID del gruppo di sicurezza con HGS  

1. Ottenere il SID del gruppo di sicurezza per gli host sorvegliati da parte dell'amministratore dell'infrastruttura ed eseguire il comando seguente per registrare il gruppo di sicurezza con HGS. 
   Eseguire nuovamente il comando se necessario per altri gruppi. 
   Specificare un nome descrittivo per il gruppo. 
   Non dovrà corrispondere al nome di gruppo di sicurezza Active Directory. 

   ```powershell
   Add-HgsAttestationHostGroup -Name "<GuardedHostGroup>" -Identifier "<SID>"
   ```

2. Per verificare se è stato aggiunto al gruppo, eseguire [Get-HgsAttestationHostGroup](https://technet.microsoft.com/library/mt652172.aspx). 


