---
title: Gestire le risorse in più foreste di Active Directory
description: Questo argomento fa parte della Guida di gestione di gestione indirizzi IP (IPAM) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b01680d4b35461ff85965781ebc60e7a613d1cb8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Gestire le risorse in più foreste di Active Directory

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come utilizzare Gestione indirizzi IP per gestire i controller di dominio, server DHCP e server DNS in più foreste di Active Directory.  
  
Per usare Gestione indirizzi IP per gestire le risorse remote foreste di Active Directory, ogni foresta che si desidera gestire deve avere due trust modo con la foresta in cui è installato Gestione indirizzi IP.  
  
Per avviare il processo di individuazione per foreste Active Directory, aprire Server Manager e fare clic su gestione indirizzi IP. Nella console del client di gestione indirizzi IP, fare clic su **configurare l'individuazione di Server**, quindi fare clic su **ottenere foreste**. Avvia un'attività in background che consente di individuare i domini e foreste trusted. Dopo aver completato il processo di individuazione, fare clic su **configurare l'individuazione di Server**, che apre la finestra di dialogo.  
  
![Configurare l'individuazione Server](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>Per il provisioning basato su gruppo Policy\ per uno scenario tra foreste Active Directory, assicurarsi di eseguire il seguente cmdlet Windows PowerShell nel server di gestione indirizzi IP e non nel dominio trusting controller di dominio. Ad esempio, se il server di gestione indirizzi IP viene aggiunto all'insieme di strutture corp.contoso.com e la foresta trusting è fabrikam.com, è possibile eseguire il seguente cmdlet Windows PowerShell nel server di gestione indirizzi IP in corp.contoso.com basate su gruppo Policy\ provisioning nella foresta fabrikam.com. Per eseguire questo cmdlet, è necessario essere un membro del gruppo Domain Admins nella foresta fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

Nel **configurare l'individuazione di Server** la finestra di dialogo, fare clic su **selezionare l'insieme di strutture**e quindi scegli la foresta che si desidera gestire con gestione indirizzi IP. Selezionare anche i domini che si desidera gestire, quindi fare clic su **Aggiungi**.

In **selezionare i ruoli server per individuare**, per ogni dominio che si desidera gestire, specificare il tipo di server da individuare. Le opzioni sono **controller di dominio**, **server DHCP**, e **server DNS**.

Per impostazione predefinita, vengono individuati i controller di dominio, server DHCP e server DNS, pertanto, se non si desidera individuare uno di questi tipi di server, assicurarsi di deselezionare la casella di controllo per l'opzione.

Nella figura di esempio precedente, il server di gestione indirizzi IP viene installato nella foresta contoso.com e il dominio radice della foresta fabrikam.com viene aggiunto per la gestione di gestione indirizzi IP. I ruoli del server selezionato consentono a gestione indirizzi IP individuare e gestire i controller di dominio, server DHCP e server DNS nel dominio radice della fabrikam.com e il dominio radice della contoso.com.

Dopo aver specificato gli insiemi di strutture, domini e i ruoli del server, fare clic su **OK**. Gestione indirizzi IP esegue l'individuazione e al termine del processo di individuazione, è possibile gestire le risorse nella foresta locale e remota.
