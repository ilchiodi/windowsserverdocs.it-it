---
title: Gestire le risorse in più foreste di Active Directory
description: Questo argomento fa parte della Guida di gestione di gestione indirizzi IP (IPAM) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2bbd303df635af314cee2126a75f0569ede2f5de
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282193"
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>Gestire le risorse in più foreste di Active Directory

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per imparare a usare Gestione indirizzi IP per gestire i controller di dominio, server DHCP e server DNS in più foreste di Active Directory.  
  
Per usare Gestione indirizzi IP per gestire le risorse nelle foreste di Active Directory remote, ogni foresta che si desidera gestire deve avere due trust bidirezionale con la foresta in cui è installato Gestione indirizzi IP.  
  
Per avviare il processo di individuazione per diverse foreste Active Directory, aprire Server Manager e fare clic su gestione indirizzi IP. Nella console del client di gestione indirizzi IP, fare clic su **configurare l'individuazione di Server**, quindi fare clic su **ottenere foreste**. Verrà avviata un'attività in background che consente di individuare i domini e foreste trusted. Dopo aver completato il processo di individuazione, fare clic su **configurare l'individuazione Server**, che apre la finestra di dialogo seguente.  
  
![Configurare l'individuazione server](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>Per criteri di gruppo\-basato il provisioning per uno scenario tra foreste Active Directory, assicurarsi di eseguire il seguente cmdlet Windows PowerShell nel server di gestione indirizzi IP e non sul dominio trusting i controller di dominio. Ad esempio, se il server di gestione indirizzi IP è unito al corp.contoso.com foresta e la foresta trusting è fabrikam.com, è possibile eseguire il seguente cmdlet Windows PowerShell nel server di gestione indirizzi IP in corp.contoso.com per criteri di gruppo\-basato il provisioning di foresta di Fabrikam.com. Per eseguire questo cmdlet, è necessario essere un membro del gruppo Domain Admins nella foresta fabrikam.com.

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

Nel **configurare l'individuazione di Server** finestra di dialogo, fare clic su **selezionare la foresta**e quindi scegliere la foresta che si desidera gestire con gestione indirizzi IP. Selezionare anche i domini che si desidera gestire e quindi fare clic su **Add**.

Nelle **selezionare i ruoli del server per individuare**, per ogni dominio che si desidera gestire, specificare il tipo di server da individuare. Le opzioni disponibili sono **controller di dominio**, **server DHCP**, e **server DNS**.

Per impostazione predefinita, vengono individuati controller di dominio, server DHCP e server DNS, pertanto se non si desidera individuare uno di questi tipi di server, assicurarsi di deselezionare la casella di controllo per tale opzione.

Nella figura di esempio precedente, il server di gestione indirizzi IP viene installato nella foresta contoso.com e viene aggiunto il dominio radice della foresta fabrikam.com per gestione indirizzi IP. I ruoli del server selezionato consentono di gestione indirizzi IP individuare e gestire i controller di dominio, server DHCP e server DNS nel dominio radice di fabrikam.com e il dominio radice di contoso.com.

Dopo aver specificato gli insiemi di strutture, domini e i ruoli del server, fare clic su **OK**. Gestione indirizzi IP esegue l'individuazione e al termine del processo di individuazione, è possibile gestire le risorse in entrambe le foreste locali e remote.
