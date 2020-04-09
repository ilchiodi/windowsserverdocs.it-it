---
title: Gestire le risorse in più foreste di Active Directory
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d86b93180708c772c052b242412bfdd1fc7e68c7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860654"
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Gestire le risorse in più foreste di Active Directory

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come utilizzare Gestione indirizzi IP per gestire i controller di dominio, i server DHCP e i server DNS in più foreste Active Directory.  
  
Per usare Gestione indirizzi IP per gestire le risorse nelle foreste Active Directory Remote, ogni foresta che si vuole gestire deve avere un trust bidirezionale con la foresta in cui è installato Gestione indirizzi IP.  
  
Per avviare il processo di individuazione per diverse foreste di Active Directory, aprire Server Manager e fare clic su Gestione indirizzi IP. Nella console del client di gestione indirizzi IP fare clic su **Configura individuazione server**e quindi fare clic su **Ottieni foreste**. Viene avviata un'attività in background che individua foreste attendibili e i rispettivi domini. Al termine del processo di individuazione, fare clic su **Configura individuazione server**. verrà visualizzata la finestra di dialogo seguente.  
  
![Configurare l'individuazione server](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>Per Criteri di gruppo provisioning basato su\-per uno scenario Active Directory tra foreste, assicurarsi di eseguire il cmdlet di Windows PowerShell seguente nel server di gestione indirizzi IP e non nei controller di dominio trusting. Ad esempio, se il server di gestione indirizzi IP viene aggiunto alla foresta corp.contoso.com e la foresta trusting è fabrikam.com, è possibile eseguire il cmdlet di Windows PowerShell seguente nel server di gestione indirizzi IP in corp.contoso.com per Criteri di gruppo provisioning basato su\-nella foresta fabrikam.com. Per eseguire questo cmdlet, è necessario essere un membro del gruppo Domain Admins nella foresta fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

Nella finestra di dialogo **Configura individuazione server** fare clic su **Seleziona foresta**, quindi scegliere la foresta che si desidera gestire con gestione indirizzi IP. Selezionare anche i domini che si desidera gestire, quindi fare clic su **Aggiungi**.

In **selezionare i ruoli del server da individuare**, per ogni dominio che si desidera gestire, specificare il tipo di server da individuare. Le opzioni sono **controller di dominio**, **server DHCP**e **server DNS**.

Per impostazione predefinita, vengono individuati i controller di dominio, i server DHCP e i server DNS. se non si desidera individuare uno di questi tipi di server, assicurarsi di deselezionare la casella di controllo relativa a tale opzione.

Nell'illustrazione di esempio precedente, il server di gestione indirizzi IP viene installato nella foresta contoso.com e viene aggiunto il dominio radice della foresta fabrikam.com per la gestione di gestione indirizzi IP. I ruoli del server selezionati consentono a gestione indirizzi IP di individuare e gestire i controller di dominio, i server DHCP e i server DNS nel dominio radice fabrikam.com e nel dominio radice contoso.com.

Dopo aver specificato le foreste, i domini e i ruoli del server, fare clic su **OK**. Gestione indirizzi IP esegue l'individuazione e, al termine dell'individuazione, è possibile gestire le risorse sia nella foresta locale che in quella remota.
